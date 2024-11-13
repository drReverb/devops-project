pipeline {
    agent any
    environment {
		MAVEN_HOME = tool 'Maven 3.9.9'
		JAVA_HOME = tool 'Java 21'
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
        // stage('Deploy to Tomcat') {
		// 	steps {
		// 		bat '''
		// 		rmdir /S /Q "%TOMCAT_HOME%\\webapps\\your-app"
		// 		xcopy /s /e /i /y /q target/*.war "%TOMCAT_HOME%\\webapps\\your-app"
		// 		'''
		// 	}
		// }
		// stage('Restart Tomcat') {
		// 	steps {
		// 		bat '''
		// 		net stop Tomcat9
		// 		net start Tomcat9
		// 		'''
		// 	}
		// }
    }
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed.'
        }
    }
}
