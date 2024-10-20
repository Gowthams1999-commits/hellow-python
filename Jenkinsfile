pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'your-dockerhub-username/webapp'
        KUBECONFIG = credentials('kubeconfig') // Jenkins credentials for K8s
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-repo/webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_HUB_REPO}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${DOCKER_HUB_REPO}:latest").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

