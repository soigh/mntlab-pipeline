node('EPBYMINW1766') {
    stage('Preparation') {
        echo "==> Preparation stage begins."
        git branch: 'amaslakou', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
    }
    
    stage('Build') {
        echo "==> Building stage begins."
        sh '/opt/gradle/gradle-4.0.1/bin/gradle build'
    }

    stage('Test'){
        echo "==> Testing stage begins."
        parallel (
                echo "==> Unit Test  stage begins."
                phase1: {sh '/opt/gradle/bin/gradle test' },
                echo "==> Jacoco Test stage begins."
                phase2: {sh '/opt/gradle/bin/gradle jacocoTestReport' },
                echo "==> Cucumber Test stage begins."
                phase3: {sh '/opt/gradle/bin/gradle cucumber' }
        )
    }
}
