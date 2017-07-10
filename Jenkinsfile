def student = "zvirinsky"

node("EPBYMINW2472"){

    stage ('Checking out') {
    git branch: "${student}", url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }

    stage('Building code') {
    sh "gradle build"
    }


    stage ('Testing code'){
      parallel (
        Branch1: {
          stage ('Cucumber'){sh "gradle cucumber"}
        },
        Branch2: {
          stage ('jacocoTestReport'){sh "gradle jacocoTestReport"}
        },
        Branch3: {
          stage ('test'){sh "gradle test"}
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
        sh "tar -xf ${student}_dsl_script.tar.gz"
        sh "tar -zcf pipeline-${student}-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile -C build/libs/ gradle-simple.jar"
        archiveArtifacts "pipeline-${student}-${BUILD_NUMBER}.tar.gz"
        sh "groovy nexus_files.groovy -t push -a pipeline-${student} -c ${BUILD_NUMBER} -r project-releases"
        }

    stage('Asking for manual approval') {
        input id: 'Manual approve', message: 'Deploy the artifact?', ok: 'Deploy'
    }

    stage('Sending status') {
        echo "AUTOMATED DEPLOYMENT is SUCCESS"
    }
}
