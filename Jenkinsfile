node {
    stage ('checkout') {
    //checking out github repository
    git branch: 'zvirinsky', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }
    stage('Build') {
    // Building code
    sh "/opt/gradle-4.0.1/bin/gradle build"
    }
}
