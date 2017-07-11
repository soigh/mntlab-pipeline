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
echo 'SUCCESS'
}
stage('Asking for manual approval') {
echo 'SUCCESS'
}
stage('Deployment') {
echo 'SUCCESS'
}
stage('Sending status') {
echo 'SUCCESS'
}
}
