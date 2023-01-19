pipeline {
    agent any
    environment {
      registry = "krishxo/jenkins-docker"
      DOCKERHUB_CREDENTIALS = credentials('dockerhub')
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
	stage('Login') {
	      steps{
		   sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
	     }
	}
        stage('Push image to DockerHUB'){
            steps{
		 bat ' docker push krishxo/jenkins-docker:$BUILD_NUMBER'   
                }
            }
        }
    }
