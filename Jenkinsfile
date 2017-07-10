node {
   stage('Preparation (Checking out)') {
		checkout([$class: 'GitSCM', branches: [[name: '*/asemirski']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline.git']]])
}
