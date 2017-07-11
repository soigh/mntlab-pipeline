node(env.SLAVE) {
	env.PATH=env.PATH+':/opt/gradle-4.0.1/bin'

stage('Preparation (Checking out)') { 
		git branch: 'pyurchuk', url: 'https://github.com/MNT-Lab/mntlab-pipeline'
}

stage('Builing code') {
     sh 'gradle build'  
}
    
stage('Testing code') {
parallel (
    stage('Cucumber Tests') {sh 'gradle cucumber' },
    stage('Jacoco Tests') {sh 'gradle jacocoTestReport' },
    stage('Unit Tests') {sh 'gradle test' }
    )
}
    post { 
        always { 
            echo 'All tests are passed successfully'
        }
    }
    stage ('Triggering job and fetching artefact after finishing') {
    
    build job: 'MNTLAB-pyurchuk-child1-build-job', parameters: [string(name: 'BRANCH_NAME', value: 'pyurchuk')]
		
	def archiveName = 'pyurchuk_dsl_script.tar.gz'
	try {
    	step($class: 'hudson.plugins.copyartifact.CopyArtifact', projectName: 'MNTLAB-pyurchuk-child1-build-job', filter: archiveName)
		} catch (none) {
    	    echo "There is no any " " + name + "" match yurchuk_dsl_script.tar.gz"
    		writeFile file: '${archiveName}'
		}
	}
}
