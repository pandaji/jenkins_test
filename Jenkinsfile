pipeline {
    agent any
    stages {
			stage('start') {
				steps{
					notifyStarted()
				}
			}

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
    			sh 'docker push localhost:37648/jh_jenkins_test:1.0'
				}
    	}
    }

	post {
    success {
				notifySuccessful()
    }

		failure {
				notifyFailed()
		}
	}
}


def notifyStarted() {
    slackSend (color: 'good', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
}

def notifySuccessful() {
    slackSend (color: 'good', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
}

def notifyFailed() {
  slackSend (color: 'danger', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
}
