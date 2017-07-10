#!groovy

node ('EPBYMINW6405'){
    stage('Build') {
     sh 'make'
    }
/*
    stage('Test') {
         sh 'make check'
    *///    junit 'reports/**/*.xml'
    /*}

    stage('Deploy') {
     sh 'make publish'  
    }
}
pipeline {
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
            echo 'I will always say Hello again!'
        }
    }
}
//}
