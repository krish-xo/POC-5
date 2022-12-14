pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    environment {
      registry = "rahilnawab/devops-integration"
      registryCredential = 'dockerhub-pwd'
      dockerImage = ''
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rahilnawab/CI']]])
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
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
                    }   
                }
            }
        }
    }
}
