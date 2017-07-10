node {
    stage ('Checkout') {
    //checking out github repository
    git branch: 'zvirinsky', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }
    stage('Build') {
    // Building code
    sh "/opt/gradle-4.0.1/bin/gradle build"
    }
    stage ('Parallel testing'){
      parallel firstBranch: {
          sh "/opt/gradle-4.0.1/bin/gradle cucumber"
      }, secondBranch: {
          sh "/opt/gradle-4.0.1/bin/gradle jacocoTestReport"
      }, thirdBranch: {
          sh "/opt/gradle-4.0.1/bin/gradle test"
      failFast: true|false
    }
}
