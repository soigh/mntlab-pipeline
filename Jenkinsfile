node {
  def student = 'akonchyts'
  stage('Preparation (Checking out)') {
    git branch: "${student}", url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
  }
  stage('Building code') {
    sh "gradle build"
  }
  stage('Testing code') {
    parallel (
      phase1: {
        stage('Unit Tests') {
          sh "gradle cucumber; echo phase1"
        }
      },
      phase2: {
        stage('Jacoco Tests') {
          sh "gradle jacocoTestReport; echo phase2"
        }
      },
      phase3: {
        stage('Cucumber Tests') {
          sh "gradle test; echo phase3"
        }
      }
    )
  }
  stage('Triggering job') {
    build job: "MNTLAB-${student}-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: "${student}")]
  }
}
