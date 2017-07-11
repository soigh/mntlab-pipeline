node {
	stage('Preparation') {
		git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: 'asemirski'
	}
    stage('Gradle Build') {
        sh "./gradle-3.5/bin/gradle buid"
    }
    stage ('Testing') {
    	parallel (
    		cucumber: {
    			stage ('cucumber') {
    				sh "./gradle-3.5/bin/gradle cucumber"
    			}
    		},
    		jacoco: {
    			stage ('jacoco') {
    				sh "./gradle-3.5/bin/gradle jacocoTestReport"
    			}
    		},
    		unit: {
    			stage ('unit test') {
    				sh "./gradle-3.5/bin/gradle test"
    			}
    		}
    	)
    }
}
    