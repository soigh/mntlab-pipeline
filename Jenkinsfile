def student = "zvirinsky"

node {
    stage ('Checking out') {
    //checking out github repository
    git branch: "${student}", url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }
    stage('Building code') {
    // Building code
    sh "/opt/gradle-4.0.1/bin/gradle build"
    }


    stage ('Testing code'){
      parallel (
        Branch1: {
          stage ('Cucumber'){sh "/opt/gradle-4.0.1/bin/gradle cucumber"}
        },
        Branch2: {
          stage ('jacocoTestReport'){sh "/opt/gradle-4.0.1/bin/gradle jacocoTestReport"}
        },
        Branch3: {
          stage ('test'){sh "/opt/gradle-4.0.1/bin/gradle test"}
        }
      )
    }
    stage("Trigger downstream and fetching artifact") {
        build job: "MNTLAB-${student}-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: "${student}")], wait: true
        step ([$class: 'CopyArtifact',
          projectName: "MNTLAB-${student}-child1-build-job",
          filter: '*.tar.gz']);
    }
    stage('Packaging and publishing results') {
        sh "tar -xf *.tar.gz"
        sh "tar -zcf pipeline-${student}-${BUILD_NUMBER}.tar.gz build/libs/gradle-simple.jar jobs.groovy Jenkinsfile"
    }


}
