node (env.SLAVE) {
	stage('Preparation') {
		git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: 'asemirski'
	}
    stage('Gradle Build') {
        sh "/opt/gradle/bin/gradle build"
    }
    stage ('Testing') {
    	parallel (
    		cucumber: {
    			stage ('cucumber') {
    				sh "/opt/gradle/bin/gradle cucumber"
    			}
    		},
    		jacoco: {
    			stage ('jacoco') {
    				sh "/opt/gradle/bin/gradle jacocoTestReport"
    			}
    		},
    		unit: {
    			stage ('unit test') {
    				sh "/opt/gradle/bin/gradle test"
    			}
    		}
    	)
    }
    stage ('Triggering job and fetching artefact after finishing') {
    	build job: 'MNTLAB-asemirski-child1-build-job', 
    		  parameters: [string(name: 'BRANCH_NAME', value: "asemirski")],
    		  wait: true
    	step([$class: 'CopyArtifact',
	          projectName: 'MNTLAB-asemirski-child1-build-job',
        	  filter: 'asemirski_dsl_script.tar.gz']) 
    		
    }
    stage ('Packaging and Publishing results') {
    	sh 'tar -xzf asemirski_dsl_script.tar.gz'
    	sh 'cp build/libs/mntlab-ci-pipeline.jar gradle-simple.jar'
    	sh 'tar -zcf pipeline-asemirski-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile gradle-simple.jar'
    	sh "curl -v --user 'admin:admin123' --upload-file ./pipeline-asemirski-${BUILD_NUMBER}.tar.gz http://192.168.123.55/repository/artifact_storage_semirski/pipeline-asemirski-${BUILD_NUMBER}.tar.gz"
    }	
    stage ('Asking for manual approval') {
    	input id: 'Manual',message: 'Are you sure want to deploy artifact?' ok: 'Yes'
    }
}
    