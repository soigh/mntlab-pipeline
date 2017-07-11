node {
	stage('Preparation') {
		git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: 'asemirski'
	}
    stage('Gradle Build') {
        sh "./gradlebin/gradle buid"
    }
    stage ('Testing') {
    	parallel (
    		cucumber: {
    			stage ('cucumber') {
    				sh "./gradle/bin/gradle cucumber"
    			}
    		},
    		jacoco: {
    			stage ('jacoco') {
    				sh "./gradle/bin/gradle jacocoTestReport"
    			}
    		},
    		unit: {
    			stage ('unit test') {
    				sh "./gradle/bin/gradle test"
    			}
    		}
    	)
    }
}
    