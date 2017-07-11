node('EPBYMINW2695') {
    stage('Preparation') {
        git branch: 'adoropei', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }

    stage('Build') {
        sh '/opt/gradle/bin/gradle build'
    }

    stage('Test') {
        echo 'Testing!!!'
        parallel (
                'Unit Tests': {sh '/opt/gradle/bin/gradle test' },
                'Jacoco Tests': {sh '/opt/gradle/bin/gradle jacocoTestReport' },
                'Cucumber Tests': {sh '/opt/gradle/bin/gradle cucumber' },
        )
    }

    stage('Trigger') {
        build job: "MNTLAB-adoropei-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: 'adoropei')], wait: true
        step ([$class: 'CopyArtifact',
               projectName: 'MNTLAB-adoropei-child1-build-job',
               filter: 'adoropei_dsl_script.tar.gz']);
    }
    stage('Packaging and Publishing') {
        sh 'tar xvf adoropei_dsl_script.tar.gz'
        sh 'ls'
        sh 'ls build'
        sh 'ls build/libs'
        sh 'cp build/libs/Pipeline.jar gradle-simple.jar'
        sh 'tar -czf pipeline-adoropei-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile gradle-simple.jar'
    }
}