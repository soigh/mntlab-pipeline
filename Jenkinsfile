node(env.SLAVE) {
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
        sh 'cp build/libs/mntlab-ci-pipeline.jar gradle-simple.jar'
        sh 'tar -czf pipeline-adoropei-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile gradle-simple.jar'
        archiveArtifacts 'pipeline-adoropei-${BUILD_NUMBER}.tar.gz'
        dir('scripts') {
            git branch: 'adoropei', url: 'https://github.com/MNT-Lab/groovy-scripts.git'
        }
        sh '/home/student/Downloads/groovy-2.4.12/bin/groovy scripts/push_pull.groovy -a push -f pipeline-adoropei-${BUILD_NUMBER}.tar.gz'
    }


    stage('Asking for manual approval') {
        timeout(time: 10, unit: 'MINUTES') {
            node {
                input id: 'Approve', message: 'Deploy this build?', ok: 'Deploy'
            }
        }
    }
    stage ('Deployment') {
        sh ' RESULT = java -jar gradle-simple.jar '
    }

    stage ('Sending status') {
        sh ' echo "SUCCESS" '
    }
}