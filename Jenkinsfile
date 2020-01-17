node {

	stage('git checkout') {
		git 'https://github.com/pandaji/jenkins_test'
	}

	stage('Build Docker Image') {
		sh 'docker build -t localhost:37648/jh_jenkins_test:1.0 .'
	}

	stage('Push Docker Image') {
		sh 'docker push localhost:37648/jh_jenkins_test:1.0'
	}
}
