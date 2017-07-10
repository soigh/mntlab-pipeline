node('EPBYMINW1374') {
    stage('check'){
	git branch: 'dsilnyagin', credentialsId: 'amazurenko4tests-passwd', url: 'https://github.com/MNT-Lab/mntlab-pipeline'
	fileExists 'build.gradle'
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
    stage('Java execute') {
    	sh "java -jar ./build/libs/mntlab-ci-pipeline.jar"
    }
    stage('Archve artifacts') {
	archiveArtifacts artifacts: 'build/', onlyIfSuccessful: true
    }
    stage('Upload') {
	sh "tar -czvf artifact-$BUILD_NUMBER.tar.gz ./build/"
	sh "curl -v --user 'admin:admin123' --upload-file artifact-$BUILD_NUMBER.tar.gz http://10.6.102.44/repository/mnt-pipeline/artifact-MNT-$BUILD_NUMBER.tar.gz"
    }
    stage('Custom') {
	sh "tree"
    }
}
