#!groovy

import groovy.json.JsonOutput
import java.util.Optional
import hudson.tasks.test.AbstractTestResultAction
import hudson.model.Actionable
import hudson.tasks.junit.CaseResult



def notifyStarted() {
    slackSend (color: 'good', message: "The build on ${currentBuild.fullDisplayName} has just *STARTED*. \n\nGood Luck! :smile:")
}

def notifySuccessful() {
    slackSend (color: 'good', message: "The build on ${currentBuild.fullDisplayName} completed *SUCCESSFULLY*. \nTotal Build Time: ${currentBuild.durationString}. \n\nWell done! :tada:")
}

def notifyFailed() {

	def testResultAction = currentBuild.rawBuild.getAction(AbstractTestResultAction.class)
	def failedTestsString = "```"

	if (testResultAction != null) {
			def failedTests = testResultAction.getFailedTests()

			if (failedTests.size() > 9) {
					failedTests = failedTests.subList(0, 8)
			}

			for(CaseResult cr : failedTests) {
					failedTestsString = failedTestsString + "${cr.getFullDisplayName()}:\n${cr.getErrorDetails()}\n\n"
			}
			failedTestsString = failedTestsString + "```"
	}

	slackSend (color: 'danger', message: "The build on ${currentBuild.fullDisplayName} *FAILED*. \nTotal Build Time: ${currentBuild.durationString}. \n\nPlease try again! :see_no_evil: \n\n*Console Msg:* \n" + failedTestsString)


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
    			sh 'docker build -t localhos:37648/jh_jenkins_test:1.0 .'
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
