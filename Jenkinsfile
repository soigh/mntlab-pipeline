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
	}
}

