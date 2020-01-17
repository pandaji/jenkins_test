pipeline {
    agent any
    stages {
    	stage('git checkout') {
				steps {
					git 'https://github.com/pandaji/jenkins_test'
				}

    	}

    	stage('Build Docker Image') {
				steps {
    			sh 'docker build -t localhost:37648/jh_jenkins_test:1.0 .'
				}
    	}

    	stage('Push Docker Image') {
				steps {
    			sh 'docker push localhos:37648/jh_jenkins_test:1.0'
				}
    	}
    }

	post {
    success {
        slackSend channel: 'test-automation',
                  color: 'good',
                  message: "The build on ${currentBuild.fullDisplayName} completed successfully."
    }

		failure {
				slackSend channel: 'test-automation',
									color: 'danger',
									message: "The build on ${currentBuild.fullDisplayName} failed."
		}
	}
}
