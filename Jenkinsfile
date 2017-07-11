node {
	stage('Preparation') {
		git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: 'kek'
	}
        stage('Gradle Build') {
           sh "gradle build"
        } 
}
    