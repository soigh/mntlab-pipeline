node {
    env.student='vulantsau'
    //customWorkspace '/opt/jenkins/master/workspace/customWorkspace'
    stage('Preparation(Checking out)') {
        checkout([$class: 'GitSCM', branches: [[name: '*/vulantsau']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline.git']]])
        }
    stage('Building code') {
        sh "gradle build"
    }
    stage ('Testing code') {
        parallel( 
            Unittests: {
                stage ('Unit Tests') {
                    sh "gradle test"
                }
            },
            jacocoTests: {
                stage ('Jacoco Tests') {
                    sh "gradle jacocoTestReport"
                    }
            },
            cucumberTests: {
                stage ('Cucumber Tests') {
                    sh "gradle cucumber"
                    }
            }
        )
    }
    stage('Triggering job and fetching artifact after finishing') {
        build job: 'MNTLAB-vulantsau-child1-build-job', parameters: [[$class: 'StringParameterValue', name: 'BRANCH_NAME', value: 'vulantsau']]
        step([$class: 'CopyArtifact', filter: 'vulantsau_dsl_script.tar.gz	', fingerprintArtifacts: true, flatten: true, projectName: 'MNTLAB-vulantsau-child1-build-job', target: ''])
    }
    stage ('Packaging and Publishing results') {
        /*a)*/ sh 'tar xvf vulantsau_dsl_script.tar.gz'
        /*b)*/ sh 'tar zvfc pipeline-${student}-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile build/libs/${JOB_NAME}.jar'
        /*c)*/ archiveArtifacts artifacts: 'pipeline-${student}-${BUILD_NUMBER}.tar.gz', allowEmptyArchive: false
        /*d)*/ nexusArtifactUploader artifacts: [[artifactId: 'task11ArtifactId', classifier: '', file: 'pipeline-'+env.student+'-${BUILD_NUMBER}.tar.gz' , type: 'tar.gz']], credentialsId: 'nexus_cred', groupId: 'task11GroupId', nexusUrl: '192.168.56.51:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'artifacts', version: '1.0'
    }
    stage ('Asking for manual approval') {
        timeout(time:5, unit:'DAYS') {
            input 'Approve deployment?' //, submitter: 'student'
        }
    }
    stage('Deployment') {
        //sh: 'java -jar build/libs/${JOB_NAME}.jar'
        if(!(sh(script: 'java -jar build/libs/${JOB_NAME}.jar', returnStdout: true)).contains("Hello World!")) {
            currentBuild.result = 'FAILURE'
        }
    }
    stage ('Sending status') {
        echo 'SUCCESS'
    }
}
