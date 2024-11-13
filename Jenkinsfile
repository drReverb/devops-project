pipeline {
    agent any
    environment {
		MAVEN_HOME = tool 'Maven 3.8.1'
		JAVA_HOME = tool 'jdk-21.0.4.7-hotspot'
		TOMCAT_HOME = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0'
	}
	tools {
		maven "${MAVEN_HOME}"
		jdk "${JAVA_HOME}"
	}
    stages {
		stage('Checkout') {
            steps {
                checkout scm
            }
        }
		stage('Build') {
            steps {
                bat "mvn clean package -DskipTests"
            }
        }
        stage('Unit Test') {
            steps {
                bat "mvn test"
            }
        }
        stage('Deploy to Tomcat') {
			steps {
				bat '''
				rmdir /S /Q "%TOMCAT_HOME%\\webapps\\your-app"
				xcopy /s /e /i /y /q target/*.war "%TOMCAT_HOME%\\webapps\\your-app"
				'''
			}
		}
		stage('Restart Tomcat') {
			steps {
				bat '''
				net stop Tomcat9
				net start Tomcat9
				'''
			}
		}
    }
    post {
        success {
            echo 'Pipeline executed successfully!'
            mail to: 'mayank.singh5@cognizant.com',
                 subject: "Jenkins Build Successful",
                 body: "The Jenkins pipeline has completed successfully and the build has been deployed to the development environment."
        }
        failure {
            echo 'Pipeline execution failed.'
            mail to: 'mayank.singh5@cognizant.com',
                 subject: "Jenkins Build Failed",
                 body: "The Jenkins pipeline has failed. Please check the logs for details."
        }
    }
}
