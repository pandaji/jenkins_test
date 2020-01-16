node {

	def img_name = "dgx:5000/jh_jenkins_test:1.0"

	stage('Build Docker Image') {
		sh 'docker build -t ${img_name} .'
	}

	stage('Push Docker Image') {
		sh 'docker push ${img_name}'
	}
}
