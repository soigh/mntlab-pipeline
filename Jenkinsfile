#!groovy

node ('EPBYMINW2471') {

    stage('Preparation (Checking out)') {
        checkout scm
        echo ('Finished: Preparation (Checking out)')
    }
    stage('Building code') {
        shell ('gradle build')
        echo ('Finished: Building code')
    }
    stage('Testing code') {
        parallel cucumber: {
            shell ('gradle cucumber')
        },
        jacoco: {
            shell ('gradle jacocoTestReport')
        },
        gradle: {
            shell ('gradle test')
        }
        echo ('Finished: Testing code')
    }
    stage('Triggering job and fetching artefact after finishing') {
        build job: 'EPBYMINW2471/MNTLAB-vtarasiuk-child1-build-job',
        parameters: [string(name: 'BRANCH_NAME', value: 'vtarasiuk')]
            echo ('Finished: Triggering job and fetching artefact after finishing')
    }
    stage('Packaging and Publishing results') {
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