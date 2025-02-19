pipeline {
    agent any

    stages{
        stage('Build Docker Image'){
            steps{
                sh 'echo "Executando o comando Docker Build"'
            }
        }
    stage('Push Docker Image'){
            steps{
                sh 'echo "Executando o comando Docker Push"'
            }
        }
    stage('Deploy no k3s'){
            steps{
                sh 'echo "Executando o comando kubectl apply"'
            }
        }        
    }
}