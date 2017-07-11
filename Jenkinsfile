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
}
