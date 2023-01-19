pipeline {
    agent any
    environment {
      registry = "krishxo/jenkins-docker"
      registryCredential = 'dockerhub-pwd'
      dockerImage = ''
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/krish-xo/POC-5.git']]])
                bat 'mvn clean'
                bat 'mvn package'
		bat 'mvn install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
			        dockerImage = docker.build registry + ":$BUILD_NUMBER"
			 }
            }
        }
        stage('Push image to DockerHUB'){
            steps{
         	script {
			withCredentials([usernamePassword(credentialsId: 'f593af9a-6979-492e-a28d-28be9c591a6d', passwordVariable: 'pwd', usernameVariable: 'dockerhub')]) {
				sh 'docker login -u krishxo-p${dockerhub}'
				sh 'docker push krishxo/jenkins-docker:$BUILD_NUMBER'
			}
                    }   
                }
            }
        }
    }
}
