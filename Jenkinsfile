node {
    stage('Git Checkout') {
    }
	stage ('Build') {
		docker.image('python:3.7.2')
		sh 'pip install -r requirements.txt'
	}
	stage ('test') {
		sh 'python3 test.py'
	}
	stage('SonarQube Analysis') {
        def scannerHome = tool 'SonarQube Scanner';
        withSonarQubeEnv(credentialsId: 'admin') {
            sh "${scannerHome}/bin/sonar-scanner \
	    -D sonar.projectKey=python"
        }
    }
    stage('Generate Tesy Report') {
			junit 'test-reports/*.xml'
	}
	stage ('Publish Artifactory') {
		withCredentials([usernamePassword(credentialsId: 'artifactory', passwordVariable: 'passwd', usernameVariable: 'user')]) {
			sh 'jjf rt upload test-reports/ python-generic-local/'
		}
	}
}
