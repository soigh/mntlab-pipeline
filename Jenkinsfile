node('EPBYMINW1766') {
    stage('Preparation') {
        echo "==> Preparation stage begins."
        git branch: 'amaslakou', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }
    
    stage('Building code') {
        echo "==> Building stage begins."
        sh '/opt/gradle/gradle-4.0.1/bin/gradle build'
    }

    stage('Testing code'){
        echo "==> Testing stage begins."
        parallel (
                phase1: {echo "==> Unit Test  stage begins."; sh '/opt/gradle/gradle-4.0.1/bin/gradle test' },
                phase2: {echo "==> Jacoco Test stage begins."; sh '/opt/gradle/gradle-4.0.1/bin/gradle jacocoTestReport' },
                phase3: {echo "==> Cucumber Test stage begins."; sh '/opt/gradle/gradle-4.0.1/bin/gradle cucumber' }
        )
    }
    stage('Triggeringg job and fetching artefact after finishing'){
        echo "==> Trigger stage begins."
        build job: "MNTLAB-amaslakou-child-1-build-job", parameters: [string(name: 'BRANCH_NAME', value: 'amaslakou' )]
        step ([$class: 'CopyArtifact',
        projectName: "MNTLAB-amaslakou-child-1-build-job",
        filter: 'amaslakou_dsl_script.tar.gz']);
  }
    stage('Packaging and Publishing') {
        sh 'tar xvf amaslakou_dsl_script.tar.gz'
        sh 'cp build/libs/mntlab-ci-pipeline.jar gradle-simple.jar'
        sh 'tar -czvf pipeline-amaslakou-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile gradle-simple.jar'
        archiveArtifacts 'pipeline-amaslakou-${BUILD_NUMBER}.tar.gz'
    }

}
    
