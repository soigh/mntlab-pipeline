node {
   stage('Preparation (Checking out)') { 
      git branch: 'mdemenkova', url: 'https://github.com/MNT-Lab/mntlab-pipeline'
   }
   stage('Building code') {  
         sh '/opt/gradle-4.0/bin/gradle clean build'        
 }
 stage ('Testing code'){
       parallel('Unit Tests':{ 
	  sh '/opt/gradle-4.0/bin/gradle cucumber' 
		},
		'Jacoco Tests':{
	  sh '/opt/gradle-4.0/bin/gradle jacocoTestReport' 
		},
		'Cucumber Tests':{
		  sh '/opt/gradle-4.0/bin/gradle test' 
		})       
  	  }   
 
  stage ('Triggering job and fetching artefact after finishing'){
     build job: 'MNTLAB-mdemenkova-child1-build-job', parameters: [string(name: 'BRANCH_NAME', value: 'mdemenkova')]

def archiveName = 'mdemenkova_dsl_script.tar.gz'
try {
    step($class: 'hudson.plugins.copyartifact.CopyArtifact', projectName: 'MNTLAB-mdemenkova-child1-build-job', filter: archiveName)
} catch (none) {
    echo 'No artifact to copy from ' + name + ' with name mdemenkova_dsl_script.tar.gz'
    writeFile file: '${archiveName}'
}

def content = readFile(archiveName).trim()
echo 'value archived'
}

    stage ('Packaging and Publishing results'){

sh 'tar -xf mdemenkova_dsl_script.tar.gz'
sh 'tar -czf mdemenkova-"${BUILD_NUMBER}".tar.gz jobs.groovy Jenkinsfile gradle-simple.jar'
nexusArtifactUploader artifacts: [[artifactId: "${BUILD_NUMBER}", classifier: 'tar.gz', file: '/target/pipeline-mdemenkova-"${BUILD_NUMBER}".tar.gz', type: "${BUILD_NUMBER}"]], credentialsId: 'admin', groupId: 'groupid', nexusUrl: '192.168.56.51:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'artifact', version: 'release'

}
stage ('Asking for manual approval'){
input 'Deploy or Abort?'
}
stage ('Deployment'){
sh 'java -jar build/libs/gradle-simple.jar'
}
stage ('Sending status'){
echo 'SUCCESS'
}
}
