pipeline {
    agent any

    stages{
        stage('Build Docker Image'){
            steps{
               dockerapp = docker.build("fornas/guia-jenkins:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
            }
        }
    stage('Push Docker Image'){
            steps{
               docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                    dockerapp.push('latest') 
                    dockerapp.push("${env.BUILD_ID}")
               }
            }
        }
    stage('Deploy no k3s'){
            steps{
                sh 'echo "Executando o comando kubectl apply"'
            }
        }        
    }
}