pipeline {
    agent any
    tools {
        maven 'Maven 3.6.3'  // or replace with Gradle if you're using Gradle
    }
    stages {
        stage('Checkout') {
			steps {
				git branch: 'dev', url: 'https://github.com/drReverb/devops-project.git'
			}
		}
        stage('Build') {
		   steps {
			  script {
				 if (fileExists('build.gradle')) {
					sh './gradlew clean build'
				 } else if (fileExists('pom.xml')) {
					sh 'mvn clean install'
				 }
			  }
		   }
		}
        stage('Unit Test') {
		   steps {
			  script {
				 if (fileExists('build.gradle')) {
					sh './gradlew test'
				 } else if (fileExists('pom.xml')) {
					sh 'mvn test'
				 }
			  }
		   }
		}
        stage('Deploy to Dev') {
		   when {
			  expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
		   }
		   steps {
			  script {
				 sh 'curl -T target/*.war "http://tomcat-user:tomcat-pass@tomcat-server:8090/manager/text/deploy?path=/your-app&update=true"'
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
