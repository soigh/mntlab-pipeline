node('EPBYMINW1374') {
    stage('check'){
	//git branch: 'dsilnyagin', credentialsId: 'amazurenko4tests-passwd', url: 'https://github.com/MNT-Lab/mntlab-pipeline'
	checkout([$class: 'GitSCM', branches: [[name: '*/dsilnyagin']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline.git']]])
	fileExists 'build.gradle'
	sh "mkdir -p ./artifacts/"
    }
    stage('Build project') {
	sh "./gradle/4.0.1/bin/gradle build"
    }
    stage('Testing') {
	parallel (
        Branch1: {
            stage ('Cucumber'){sh "./gradle/4.0.1/bin/gradle cucumber"}
        },
        Branch2: {
            stage ('jacocoTestReport'){sh "./gradle/4.0.1/bin/gradle jacocoTestReport"}
        },
        Branch3: {
            stage ('test'){sh "./gradle/4.0.1/bin/gradle test"}
        }
        )
    }
    stage('Job trigger') {
	build job: 'EPBYMINW1374/MNTLAB-dsilnyagin-child1-build-job', parameters: [string(name: 'BRANCH_NAME', value: 'dsilnyagin')]
    }
    stage('Archve artifacts') {
	archiveArtifacts artifacts: 'build/', onlyIfSuccessful: true
    }
    stage('Upload') {
	sh "tar -czvf ./artifacts/pipeline-'$BUILD_NUMBER'.tar.gz ./build/"
	sh "curl -v --user 'admin:admin123' --upload-file ./artifacts/pipeline-'$BUILD_NUMBER'.tar.gz http://10.6.102.44/repository/mnt-pipeline/pipeline.tar-'$BUILD_NUMBER'.tar.gz"
    }
    stage('Approve') {
	input 'Deploy!'
    }
    stage('Java execute') {
    	sh "java -jar ./build/libs/mntlab-ci-pipeline.jar"
    }
    stage('End') {
	echo "Success"
    }
}
