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
                'Unit Tests': {sh 'gradle test' },
                'Jacoco Tests': {sh 'gradle jacocoTestReport' },
                'Cucumber Tests': {sh 'gradle cucumber' },
        )
    }

    stage('Deploy') {
        echo 'Deploying!!!'
    }
}