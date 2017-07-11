node(env.SLAVE) {
  def student = 'akonchyts'
  stage('Preparation (Checking out)') {
    git branch: "${student}", url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
  }
  stage('Building code') {
    sh "gradle build"
  }
  stage('Testing code') {
    parallel (
      phase1: { sh "gradle cucumber; echo 'Cucumber Tests'" },
      phase2: { sh "gradle jacocoTestReport; echo 'Jacoco Tests'" },
      phase3: { sh "gradle test; echo 'Unit Tests'" }
    )
  }
  stage('Triggering job and fetching artifact') {
    build job: "MNTLAB-${student}-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: "${student}")]
    step ([$class: 'CopyArtifact',
          projectName: "MNTLAB-${student}-child1-build-job",
          filter: "${student}_dsl_script.tar.gz"]);
  }
  stage('Packaging and Publishing results') {
    sh "tar -xzf ${student}_dsl_script.tar.gz jobs.groovy"
    sh "tar -czf pipeline-${student}-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile -C build/libs/ gradle-simple.jar"
    archiveArtifacts "pipeline-${student}-${BUILD_NUMBER}.tar.gz"
    sh "groovy pullpushArtifacts.groovy -p push -b pipeline-${student} -c ${BUILD_NUMBER}"
  }
  stage('Asking for manual approval') {
    timeout(time: 10, unit: 'MINUTES') {
      input id: 'Manual approve', message: "Are you sure that pipeline-${student}-${BUILD_NUMBER}.tar.gz artifact should be deployed", ok: 'Deploy'
    }
  }
  stage('Deployment') {
    sh "java -jar build/libs/gradle-simple.jar"
  }
  stage('Sending status') {
    echo 'SUCCESS. All stages have been completed successfully'
  }
}
