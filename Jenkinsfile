node {
	stage('Preparation') {
		git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: 'asemirski'
	}
    stage('Gradle Build') {
        sh "gradle build"
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
    