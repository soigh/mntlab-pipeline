node(env.SLAVE) {
stage('Preparation (Checking out)') {
git branch: 'hpashuto', url: 'https://github.com/MNT-Lab/mntlab-pipeline.git'
}
stage('Building code') {
sh '/opt/gradle/gradle-4.0/bin/gradle build'
}
stage('Testing code') {
parallel (
'Unit Tests': {sh '/opt/gradle/gradle-4.0/bin/gradle test'}, 
'Jacoco Tests': {sh '/opt/gradle/gradle-4.0/bin/gradle jacocoTestReport'}, 
'Cucumber Tests': {sh '/opt/gradle/gradle-4.0/bin/gradle cucumber'},
    )
}
stage('Triggering job and fetching artefact after finishing') {
build job: 'EPBYMINW2033/MNTLAB-hpashuto-child1-build-job', parameters: [string(name: 'BRANCH_NAME', value: 'hpashuto')]
		step ([$class: 'CopyArtifact',
		          projectName: 'EPBYMINW2033/MNTLAB-hpashuto-child1-build-job',
filter: 'hpashuto_dsl_script.tar.gz']);
}
stage('Packaging and Publishing results') {
sh 'tar -xzf hpashuto_dsl_script.tar.gz jobs.groovy'
sh 'ls -la'
sh 'ls build/libs'
sh 'tar -czf pipeline-hpashuto-"${BUILD_NUMBER}".tar.gz jobs.groovy Jenkinsfile -C build/libs gradle-simple.jar'
sh 'ls -la'
echo 'nexus'
sh '/home/student/Desktop/soft/groovy-2.4.12/bin/groovy pull.groovy -p push -a pipeline-hpashuto-${BUILD_NUMBER}.tar.gz'
archiveArtifacts 'pipeline-hpashuto-${BUILD_NUMBER}.tar.gz'
}
stage('Asking for manual approval') {
timeout(10) {
input id: 'Deployapprove', message: 'DEPLOY?', ok: 'deploy'
}
}
stage('Deployment') {
sh 'java -jar build/libs/gradle-simple.jar'
}
stage('Sending status') {
echo 'SUCCESS'
}
}
