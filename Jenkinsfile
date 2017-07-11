node ('EPBYMINW2468') {
	env.PATH=env.PATH+":/opt/gradle/gradle-4.0.1/bin:/opt/groovy-2.4.12/bin"
	stage('Preparation') {
		git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: 'asemirski'
	}
    stage('Gradle Build') {
        sh "gradle buid"
    }
    stage ('Testing') {
    	parallel (
    		cucumber: {
    			stage ('cucumber') {
    				sh "gradle cucumber"
    			}
    		},
    		jacoco: {
    			stage ('jacoco') {
    				sh "gradle jacocoTestReport"
    			}
    		},
    		unit: {
    			stage ('unit test') {
    				sh "gradle test"
    			}
    		}
    	)
    }
}
    