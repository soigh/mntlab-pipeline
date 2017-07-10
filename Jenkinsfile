node('EPBYMINW2695') {
    stage('Preparation') {
        git branch: 'adoropei', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }

    stage('Build') {
        sh '/opt/gradle/bin/gradle build'

    }

    stage('Deploy') {
        echo 'Deploying!!!'
    }
}