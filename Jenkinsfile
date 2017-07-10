#!groovy

node ('EPBYMINW6405'){
    stage('Build') {
     sh 'make'
    }

stage('Builing code') {
     sh 'gradle build'  
    }

/*pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
} */

stage('Testing code') {
parallel (
    stage('Cucumber Tests') {sh 'gradle cucumber' },
    stage('Jacoco Tests') {sh 'gradle jacocoTestReport' },
    stage('Unit Tests') {sh 'gradle test' }
    )
}
    post { 
        always { 
            echo 'All tests are passed successfully'
        }
    }
}
//}
