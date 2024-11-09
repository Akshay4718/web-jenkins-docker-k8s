pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t akshay1867/web-jenkins-docker-k8s .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                        sh 'docker push akshay1867/web-jenkins-docker-k8s:latest'
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // List files to verify the file location
                sh 'ls -R $WORKSPACE'
                
                // Update path as needed
                sh 'kubectl apply -f $WORKSPACE/k8s-deployment/k8s-deployment.yaml'
            }
        }
    }
}
