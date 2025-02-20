pipeline {
    agent any // O pipeline pode ser executado em qualquer agente disponível
        // CI - Continuous Integration
    stages{
        stage('Build Docker Image'){ // Etapa de construção da imagem Docker
            steps{
                script {
               // Cria a imagem Docker utilizando o Dockerfile localizado em ./src
               // A imagem é nomeada com base no nome do repositório e o ID da build do Jenkins
                    dockerapp = docker.build("marcelofornas/pipeline-jenkins:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage('Push Docker Image'){ // Etapa de envio da imagem Docker para o Docker Hub
            steps{
                script{
                    // Configura o Docker para utilizar o registro do Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                    // Envia a imagem Docker para o repositório, marcando-a com 'latest' e com o ID da build    
                    dockerapp.push('latest') 
                    dockerapp.push("${env.BUILD_ID}")
                    }
               }
            }
        }

        // CD - Continuous Deployment
        stage('Deploy no k3s') { // Etapa de implantação no cluster k3s
            environment {
                // Define uma variável de ambiente para a versão da imagem baseada no ID da build
                tag_version = "${env.BUILD_ID}"
            }
            steps{
                // Usa credenciais para acessar o cluster k3s
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    // Substitui o placeholder {{tag}} pelo ID da build no arquivo de deployment do k3s
                    sh 'sed -i "s/{{tag}}/$tag_version/g" k8s/deployment.yaml'
                    // Aplica a configuração do deployment no cluster Kubernetes
                    sh 'kubectl apply -f k8s/deployment.yaml'
                }
            }        
        }
    }
}