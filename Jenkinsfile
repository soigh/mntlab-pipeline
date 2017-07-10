#!groovy
node('EPBYMINW2468') {
	env.PATH=env.PATH+":/opt/gradle/gradle-4.0.1/bin"
	stage('Preparation (Checking out)') {
		//git branch: 'yshchanouski', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
		checkout([$class: 'GitSCM', branches: [[name: '*/yshchanouski']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline.git']]])
	}
	stage('Building code') {
        	sh 'gradle build'
	}
	stage 'Testing code' 
		parallel (
			'Cucumber Tests': {sh 'gradle cucumber' },
                        'Jacoco Tests': {sh 'gradle jacocoTestReport' },
                        'Unit Tests': {sh 'gradle test' }
		)
	
        stage('Triggering job and fetching artefact after finishing') {
                build job: 'EPBYMINW2468/MNTLAB-yshchanouski-child1-build-job', parameters: [string(name: 'BRANCH_NAME', value: 'yshchanouski')]
        }
}

