/* #!groovy

node {
    stage('Build') {
     sh 'make'
    }

    stage('Test') {
         sh 'make check'
    *///    junit 'reports/**/*.xml'
    /*}

    stage('Deploy') {
     sh 'make publish'  
    }
}*/

pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
        }
    }
}
