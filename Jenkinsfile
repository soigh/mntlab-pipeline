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
      parallel (
        Branch1: {stage ('Cucumber'){sh "/opt/gradle-4.0.1/bin/gradle cucumber"}}
        Branch2: {stage ('jococoTestReport'){sh "/opt/gradle-4.0.1/bin/gradle jococoTestReport"}}
        Branch3: {stage ('test'){sh "/opt/gradle-4.0.1/bin/gradle test"}}
      )
    }
}
