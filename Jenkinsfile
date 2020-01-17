
def notifyStarted() {
    slackSend (color: 'warning', message: "The build on ${currentBuild.fullDisplayName} has just *STARTED*. \n\nGood Luck! :smile:")
}

def notifySuccessful() {
    slackSend (color: 'good', message: "The build on ${currentBuild.fullDisplayName} completed *SUCCESSFULLY*. \nTotal Build Time: ${currentBuild.durationString}. \n\nWell done! :tada:")
}

def notifyFailed() {
	slackSend (color: 'danger', message: "The build on ${currentBuild.fullDisplayName} *FAILED*. \nTotal Build Time: ${currentBuild.durationString}. \n\nPlease try again! :see_no_evil:")
}



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
