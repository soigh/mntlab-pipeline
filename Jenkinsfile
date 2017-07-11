node (env.SLAVE) {
       def student = 'ndolya'
       env.PATH=env.PATH +":/home/student/groovy-2.4.12/bin:/home/student/gradle/gradle-4.0.1/bin"

 stage('Preparation (Checking out)') {
       git([url: 'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: "${student}")
         }

 stage('Building code') {
              sh "gradle build"
           }
 stage('Testing') {

    parallel (
     'firstTest': { sh "gradle cucumber" },
     'secondTest' : { sh "gradle jacocoTestReport" },
     'thirdTest' : {  sh "gradle test" }
      )
 }

 stage ('Triggering job and fetching artefact after finishing') {

 build job: "EPBYMINW1969/MNTLAB-${student}-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: "${student}")]
step([$class: 'CopyArtifact',
      filter: "${student}_dsl_script.tar.gz",
      projectName: "EPBYMINW1969/MNTLAB-${student}-child1-build-job" ])
      }

}
