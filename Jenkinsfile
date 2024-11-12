pipeline {
    agent any
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }
	environment {
        CATALINA_HOME='C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0'
    }
    stages {
        /*stage('Checkout') {
		//	steps {
		//		git branch: 'dev', url: 'https://github.com/drReverb/devops-project.git'
		//	}
		//}*/
        /*stage('Build') {
		   steps {
			  script {
				 if (fileExists('build.gradle')) {
					sh './gradlew clean build'
				 } else if (fileExists('pom.xml')) {
					sh 'mvn clean install'
				 }
			  }
		   }
		}*/
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
                script {
                    bat 'copy "target\\*.war" "%CATALINA_HOME%\\webapps\\"'
                }
            }
        }
        stage('Restart Tomcat') {
            steps {
                script {
                    bat "net stop Tomcat9"
                    bat "net start Tomcat9"
                }
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
