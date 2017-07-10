#!groovy
node('EPBYMINW2468') {
	env.PATH=env.PATH+":/opt/gradle/gradle-4.0.1/bin"
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
		archiveArtifacts 'pipeline-yshchanouski-"${BUILD_NUMBER}".tar.gz'
		
}

