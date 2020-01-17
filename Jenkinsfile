#!groovy

import groovy.json.JsonOutput
import java.util.Optional
import hudson.tasks.test.AbstractTestResultAction
import hudson.model.Actionable
import hudson.tasks.junit.CaseResult



def notifyStarted() {
    slackSend (color: 'good', message: "The build on ${currentBuild.fullDisplayName} has just *started*. \n Good Luck! :smile:")
}

def notifySuccessful() {
		def msg = "The build on ${currentBuild.fullDisplayName} completed *successfully*. \n" +
						  "Total Build Time: ${currentBuild.durationString}. \n" +
							"Well done! :tada:"

    slackSend (color: 'good', message: ${msg})
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

	def msg = "The build on ${currentBuild.fullDisplayName} *failed*. \n" +
						"Total Build Time: ${currentBuild.durationString}. \n" +
						"Please try again! :see_no_evil: \n\n" +
						"*Console Msg:* \n" +
						failedTestsString
  slackSend (color: 'danger', message: ${msg})


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
