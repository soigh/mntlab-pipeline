node(env.SLAVE) {
	env.PATH=env.PATH+":/opt/gradle-4.0.1/bin:/opt/groovy-2.4.12/bin"

stage('Preparation (Checking out)') { 
		git branch: 'pyurchuk', url: 'https://github.com/MNT-Lab/mntlab-pipeline'
}

stage('Builing code') {
     sh "gradle build"  
}
    
stage('Testing code') {
parallel (
    'Cucumber Tests': { sh "gradle cucumber" },
    'Jacoco Tests': { sh "gradle jacocoTestReport" },
    'Unit Tests': { sh "gradle test" }
    )
}

stage ('Triggering job and fetching artefact after finishing') {
build job: "EPBYMINW6405/MNTLAB-pyurchuk-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: 'pyurchuk')]
echo WORKSPACE
step(
    [$class: 'CopyArtifact',
    filter: "pyurchuk_dsl_script.tar.gz",
    projectName: "MNTLAB-pyurchuk-child1-build-job" ])
    }

stage ('Packaging and Publishing results') {
    sh 'tar -xf pyurchuk_dsl_script.tar.gz jobs.groovy'
    sh 'tar -czf pipeline-pyurchuk-"${BUILD_NUMBER}".tar.gz jobs.groovy Jenkinsfile -C build/libs gradle-simple.jar'
    archiveArtifacts 'pipeline-pyurchuk-${BUILD_NUMBER}.tar.gz'
    sh 'groovy pull-push.groovy -p push -a pipeline-pyurchuk-${BUILD_NUMBER}.tar.gz'    
}
}
