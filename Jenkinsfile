#!groovy

node ('EPBYMINW2471') {

    stage('Preparation (Checking out)') {
        checkout scm
        echo ('Finished: Preparation (Checking out)')
    }
    stage('Building code') {
        sh ("/home/student/.sdkman/candidates/gradle/4.0.1/bin/gradle build")
        echo ('Finished: Building code')
    }
    stage('Testing code') {
        parallel cucumber: {
            sh ("/home/student/.sdkman/candidates/gradle/4.0.1/bin/gradle cucumber")
        },
        jacoco: {
            sh ("/home/student/.sdkman/candidates/gradle/4.0.1/bin/gradle jacocoTestReport")
        },
        gradle: {
            sh ("/home/student/.sdkman/candidates/gradle/4.0.1/bin/gradle test")
        }
        echo ('Finished: Testing code')
    }
    stage('Triggering job and fetching artefact after finishing') {
        build job: 'EPBYMINW2471/MNTLAB-vtarasiuk-child1-build-job',
        parameters: [string(name: 'BRANCH_NAME', value: 'vtarasiuk')]
        step ([$class: 'CopyArtifact',
                  projectName: 'EPBYMINW2471/MNTLAB-vtarasiuk-child1-build-job',
                  filter: 'vtarasiuk_dsl_script.tar.gz']);
            echo ('Finished: Triggering job and fetching artefact after finishing')
    }
    stage('Packaging and Publishing results') {
            sh ("tar -xzf vtarasiuk_dsl_script.tar.gz && cp build/libs/gradle-simple.jar ./")
            archiveArtifacts artifacts: 'gradle-simple.jar, Jenkinsfile, jobs.groovy', onlyIfSuccessful: true
            sh ("ls -l build/libs/")
            echo ('Finished: Packaging and Publishing results')
    }
    stage('Asking for manual approval') {
                echo ('Finished: Asking for manual approval')
    }
    stage('Deployment') {
            echo ('Finished: Deployment')
    }
    stage('Sending status') {
            echo ('Finished: Sending status')
    }
}