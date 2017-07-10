node('EPBYMINW1374') {
    stage('check'){
	git branch: 'dsilnyagin', credentialsId: 'amazurenko4tests-passwd', url: 'https://github.com/MNT-Lab/mntlab-pipeline'
	fileExists 'build.gradle'
    }
    stage('Build project') {
	sh "./gradle/4.0.1/bin/gradle clean build"
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
	tar -czvf artifact-$BUILD_NUMBER.tar.gz ./build/
    }
    
    stage('Custom') {
	sh "tree"
    }
}
