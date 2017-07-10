node ('EPBYMINW2471') {

    stage('Preparation (Checking out)') {
        checkout scm
        echo ('Stage: Preparation (Checking out)')
    }
    stage('Building code') {
        sh 'gradle build'
        echo ('Stage: Building code')
    }
    stage('Testing code') {
        parallel cucumber: {
            sh 'gradle cucumber'
        },
        jacoco: {
            sh 'gradle jacocoTestReport'
        },
        gradle: {
            sh 'gradle test'
        }
        echo ('Stage: Testing code')
    }
    stage('Triggering job and fetching artefact after finishing') {
            echo ('Stage: Triggering job and fetching artefact after finishing')
    }
    stage('Packaging and Publishing results') {
            echo ('Stage: Packaging and Publishing results')
    }
    stage('Asking for manual approval') {
                echo ('Stage: Asking for manual approval')
    }
    stage('Deployment') {
            echo ('Stage: Deployment')
    }
    stage('Sending status') {
                echo ('Stage: Sending status')
    }
}