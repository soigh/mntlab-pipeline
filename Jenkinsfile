node {
    stage ('Checkout') {
    //checking out github repository
    git branch: 'zvirinsky', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }
    stage('Build') {
    // Building code
    sh "/opt/gradle-4.0.1/bin/gradle build"
    }


    stage ('Testing code'){
      parallel {
        stage ('Cucumber'){sh "/opt/gradle-4.0.1/bin/gradle cucumber"}
        stage ('jococoTestReport'){sh "/opt/gradle-4.0.1/bin/gradle jococoTestReport"}
        stage ('test'){sh "/opt/gradle-4.0.1/bin/gradle test"}
      }
    }
}    
