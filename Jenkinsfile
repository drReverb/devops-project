pipeline {
    agent any
    environment {
		MAVEN_HOME = tool 'Maven 3.8.1' // Update with your Maven installation name
		JAVA_HOME = tool 'jdk-21.0.4.7-hotspot' // Update with your JDK installation
		name TOMCAT_HOME = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 11.0_Tomcat11_Temp' // Update with your Tomcat installation path
	}
	tools {
		maven "${MAVEN_HOME}"
		jdk "${JAVA_HOME}"
	}
    stages {
		stage('Checkout') {
            steps {
                // Checkout code from Git
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
				// Clean up previous deployment and copy new build files to Tomcat
				bat '''
				rmdir /S /Q "%TOMCAT_HOME%\\webapps\\your-app"
				xcopy /s /e /i /y /q target/*.war "%TOMCAT_HOME%\\webapps\\your-app"
				'''
			}
		}
		stage('Restart Tomcat') {
			steps { // Restart Tomcat service
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
