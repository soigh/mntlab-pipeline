#!groovy
node('EPBYMINW2466') {

stage('Checking out') {
git branch: 'akarzhou', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
	}	

stage ('Building code') {
sh "./gradle/bin/gradle build"
}

stage ('Testing code'){
      parallel (
        firstBranch: {
          stage ('Cucumber')
		{
			sh "./gradle/bin/gradle cucumber"
		}
        },
        secondBranch: {
          stage ('test')
		{
			sh "./gradle/bin/gradle test"
		}
        },
        thirdBranch: {
          stage ('jacocoTestReport')
		{
			sh "./gradle/bin/gradle jacocoTestReport"
		}
        }
      )
}

stage('Triggering job and fetching artefact after finishing') {
	build job: 'MNTLAB-akarzhou-child1-build-job', parameters: [string(name: 'BRANCH_NAME', value: 'akarzhou')]
	step ([$class: 'CopyArtifact',
	        projectName: 'MNTLAB-akarzhou-child1-build-job',
        	filter: 'akarzhou_dsl_script.tar.gz']);
}
stage('Packaging and Publishing results') {
	sh "tar -xzf akarzhou_dsl_script.tar.gz jobs.groovy"
	sh "tar -czf pipeline-akarzhou-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile"
	archiveArtifacts artifacts: 'pipeline-akarzhou-${BUILD_NUMBER}.tar.gz'
	nexusArtifactUploader artifacts: [[artifactId: "${BUILD_NUMBER}", classifier: 'tar.gz', file: '/target/pipeline-akarzhou-"${BUILD_NUMBER}".tar.gz', type: "${BUILD_NUMBER}"]], credentialsId: 'admin', groupId: 'groupid', nexusUrl: '192.168.56.24:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'artifact', version: 'release'
	}
	
}

