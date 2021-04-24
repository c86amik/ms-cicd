pipeline {
  environment{
	registry = "amikchak/gokoders"
	registryCredential = 'cf6545c7-e532-4ce4-a85b-a6a58ee121fa'
	flightProxyImage = ''
	flightRegistrationImage = ''
	flightSearchImage = ''
	flightBookImage = ''
    flightLoginImage = ''
  }
  agent any
    tools {
      maven 'M2_HOME'
      jdk 'JAVA_HOME'
    }
    stages {      
        stage('SonarQube analysis') {
			steps {
				withSonarQubeEnv('sonar') {
                    // Optionally use a Maven environment you've configured already
                    sh 'mvn clean package sonar:sonar -DskipTests=true'
                }
			}
		}
		stage('Build Docker Image') {
			steps{
				script {
					flightProxyImage = docker.build("${registry}:flight-proxy-service-0.6", "./flight-proxy-service")
					flightRegistrationImage = docker.build("${registry}:flight-registration-service-0.6", "./flight-registration-service")
					flightSearchImage = docker.build("${registry}:flight-search-service-0.6", "./flight-search-service")
					flightBookImage = docker.build("${registry}:flight-book-service-0.6", "./flight-book-service")
					flightLoginImage = docker.build("${registry}:flight-login-service-0.6", "./flight-login-service")
				}
			}
		}
		stage('Deploy Docker Image') {
           steps {
               script {
                 docker.withRegistry('', registryCredential) {
					flightProxyImage.push()
					sh "docker rmi -f amikchak/gokoders:flight-proxy-service-0.6"
					}
				 docker.withRegistry('', registryCredential) {
					flightRegistrationImage.push()
					sh "docker rmi -f amikchak/gokoders:flight-registration-service-0.6"
					}
				 docker.withRegistry('', registryCredential) {
					flightSearchImage.push()
					sh "docker rmi -f amikchak/gokoders:flight-search-service-0.6"
					}
				 docker.withRegistry('', registryCredential) {
					flightBookImage.push()
					sh "docker rmi -f amikchak/gokoders:flight-book-service-0.6"
					}
				 docker.withRegistry('', registryCredential) {
					flightLoginImage.push()
					sh "docker rmi -f amikchak/gokoders:flight-login-service-0.6"
					}
				}
			}
		}
		stage('Build on K8S in AWS') {
            steps {           
                    sh 'pwd'
                    sh 'cp -R helm/* .'
					sh 'ls -ltr'
                    sh 'pwd'
                    sh '/usr/local/bin/helm upgrade --install flightapp flightapp --set image.repository=registry.hub.docker.com/amikchak/gokoders --set image.tag=:flight-proxy-service-0.6'
              			
            }           
        }
	}
}
