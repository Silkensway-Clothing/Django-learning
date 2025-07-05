pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-docker-registry/django-test:${BUILD_NUMBER}"

    }
    stages {
        stage('checkout') {
            steps {
                git branch: 'main', url: 'https://github.com'
            }
        }
        stage('Build Doker image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://url', 'docker-credentails-id') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
        stage('Deploy to kubernetes') {
            steps {
                script {
                    sh """
                    kubectl apply -f k8s/pvc.yml
                    kubectl apply -f k8s/deployment.yml
                    kubectl apply -f k8s/service.yml
                    kubectl apply -f k8s/ingress.yml
                }
            }
        }
     }
}