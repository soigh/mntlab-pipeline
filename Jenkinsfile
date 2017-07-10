node (env.SLAVE) {
       env.PATH=env.PATH +":/home/student/groovy-2.4.12/bin:/home/student/gradle/gradle-4.0.1/bin"

  stage('Preparation (Checking out)') {
       git([url: 'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: 'ndolya'])
         }

    stage('Building code') {
              sh "gradle build"
           }


  parallel(firstTask: {
    stage 'Unit Tests' {
        sh "gradle cucumber"
       }

        }, secondTask: {
           stage 'Jacoco Tests'{
          sh "gradle jacocoTestReport"
       }

        }, thirdTask: {
          stage 'Cucumber Tests'{
            sh "gradle test"
          }
    }
  )
}