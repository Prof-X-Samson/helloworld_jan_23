pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '076268473962.dkr.ecr.us-east-1.amazonaws.com/devop_repository'
    registryCredential = 'aws cli-user'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Prof-X-Samson/helloworld_jan_23.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}

