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
	sh "curl -v --user 'admin:admin123' --upload-file ./pipeline-akarzhou-${BUILD_NUMBER}.tar.gz http://192.168.56.24:8081/repository/Artifact-storage/pipeline-akarzhou-${BUILD_NUMBER}.tar.gz"
	}
stage('Asking for manual approval') {
timeout(time: 120, unit: 'SECONDS') {
        input message: 'Do you want to release this build?', ok: 'Yes' 
//              parameters: [[$class: 'BooleanParameterDefinition',
//                            defaultValue: false,
//                            description: 'Ticking this box will do a release',
//                            name: 'Release']]
    }
}
stage('Deployment') {
	sh "java -jar build/libs/gradle-simple.jar"
	}
stage('Sending status') {
	sh 'echo "SUCCESS"'
	}
}

