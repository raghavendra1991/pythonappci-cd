node {
    stage('Git Checkout') {
    }

	stage ('clean') {
		/* Cleaning Workspace Stage Started */
        bat 'rmdir /s /q test-reports'
    }
		
	stage ('test') {
	    /* Test Stage Started */
		sh 'python3 test.py'
	}
	stage('SonarQube Analysis') {
	    /* Static Code Analysis */
        def scannerHome = tool 'SonarQube Scanner';
        withSonarQubeEnv(credentialsId: 'admin') {
            sh "${scannerHome}/bin/sonar-scanner \
	        -D sonar.projectKey=python"
        }
    }
    stage('Generate Test Report') {
		/* Generate Text Report */
		junit 'test-reports/*.xml'
	}
	
	stage ('Publish Artifactory') {
		/* Publish Report to JFrog Artifacts */
		withCredentials([usernamePassword(credentialsId: 'artifactory', passwordVariable: 'passwd', usernameVariable: 'user')]) {
			sh 'jf rt upload test-reports/ python-generic-local/'
		}
	}
}
