pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-java-app"
        REGISTRY = "docker.io/raja1021"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Kr1021/ci-cd-java-microservices.git'
            }
        }

        stage('Code Quality') {
            steps {
                echo 'Running SonarQube checks...'
                // Example: withSonarQubeEnv('SonarQube') { sh 'mvn sonar:sonar' }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                    docker.withRegistry('', 'docker-hub-credentials') {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
