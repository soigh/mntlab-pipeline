node {
    stage('check'){
	    git branch: 'dsilnyagin', credentialsId: 'amazurenko4tests-passwd', url: 'https://github.com/MNT-Lab/mntlab-pipeline'
	    fileExists 'build.gradle'
	}
    stage('step1'){
	    echo "he-he"
	}
}
