#!groovy
node('EPBYMINW2468') {
	stage('Preparation (Checking out)') {
		//git branch: 'yshchanouski', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
		checkout([$class: 'GitSCM', branches: [[name: '*/yshchanouski']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline.git']]])
	}
	stage('Building code') {
		sh 'environment PATH=/opt/gradle/gradle-4.0.1/bin:$PATH'
        	sh 'gradle build'
	}
	stage('Testing code') {
		parallel (
			stage('Cucumber Tests') {sh 'gradle cucumber' },
                        stage('Jacoco Tests') {sh 'gradle jacocoTestReport' },
                        stage('Unit Tests') {sh 'gradle test' }
		)
		
	}
        stage('Triggering job and fetching artefact after finishing') {
                build job: 'EPBYMINW2468/MNTLAB-yshchanouski-child1-build-job', parameters: [string(name: 'BRANCH_NAME', value: 'yshchanouski')]
		
        }
}
