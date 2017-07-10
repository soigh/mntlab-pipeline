EPBYMINW2470 {
    //customWorkspace '/opt/jenkins/master/workspace/customWorkspace'
        
        //stage('Preparation(Checking out)') {
        //    checkout([$class: 'GitSCM', branches: [[name: '*/vulantsau']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline.git']]])
        //}
        /*
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
        }*/
        stage('Triggering job and fetching artifact after finishing') {
             build job: 'MNTLAB-vulantsau-child1-build-job', parameters: [[$class: 'StringParameterValue', name: 'BRANCH_NAME', value: 'vulantsau']]
             step([$class: 'CopyArtifact', filter: 'vulantsau_dsl_script.tar.gz	', fingerprintArtifacts: true, flatten: true, projectName: 'MNTLAB-vulantsau-child1-build-job', target: ''])
        }
        stage ('Packaging and Publishing results') {
            sh 'tar czf vulantsau_dsl_script.tar.gz'
        }

}
