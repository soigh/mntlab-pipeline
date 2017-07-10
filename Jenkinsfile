#!groovy
node {
stage('Build')	{
sh '/opt/gradle/bin/gradle build'
     }
stage('Testing code')
	parallel firstBranch: {
	stage ('Cucumber')
		{
			sh '/opt/gradle/bin/gradle cucumber'
		}
	}
	parallel secondBranch: {
        stage ('Unit Tests')
                {
                        sh '/opt/gradle/bin/gradle test'
                }
        }
	parallel thirdBranch: {
        stage ('Cucumber Tests')
                {
                        sh '/opt/gradle/bin/gradle jacocoTestReport'
                }
        }
}

