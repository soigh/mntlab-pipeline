#!groovy
node('"${SLAVE}"') {
	env.PATH=env.PATH+":/opt/gradle/gradle-4.0.1/bin:/opt/groovy-2.4.12/bin"
	stage 'Preparation (Checking out)'
		//git branch: 'yshchanouski', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
		checkout([$class: 'GitSCM', branches: [[name: '*/yshchanouski']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline.git']]])
	
	stage 'Building code'
        	sh 'gradle build'
	
	stage 'Testing code' 
		parallel (
			'Cucumber Tests': {sh 'gradle cucumber' },
                        'Jacoco Tests': {sh 'gradle jacocoTestReport' },
                        'Unit Tests': {sh 'gradle test' }
		)


        stage 'Triggering job and fetching artefact after finishing' 
                build job: 'EPBYMINW2468/MNTLAB-yshchanouski-child1-build-job', parameters: [string(name: 'BRANCH_NAME', value: 'yshchanouski')]
		echo WORKSPACE
		step ([$class: 'CopyArtifact',
		          projectName: 'MNTLAB-yshchanouski-child1-build-job',
		          filter: 'yshchanouski_dsl_script.tar.gz']);
	
	stage 'Packaging and Publishing results'
		sh 'tar -xzf yshchanouski_dsl_script.tar.gz jobs.groovy'
	 	sh 'ls build/libs'
		sh 'tar -czf pipeline-yshchanouski-"${BUILD_NUMBER}".tar.gz jobs.groovy Jenkinsfile -C build/libs gradle-simple.jar'
		archiveArtifacts 'pipeline-yshchanouski-${BUILD_NUMBER}.tar.gz'
		sh 'groovy pull-push.groovy -p push -a pipeline-yshchanouski-${BUILD_NUMBER}.tar.gz'
	stage 'Asking for manual approval'
		input id: 'Answer', message: 'Approve this build?', ok: 'Approve'
	stage 'Deployment'
		sh 'java -jar build/libs/gradle-simple.jar'
	stage 'Send status'
		echo 'SUCCESS'
}

