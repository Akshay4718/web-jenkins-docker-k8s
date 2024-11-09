pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE = "akshay1867/react-jenkins-docker-k8s"
    }
    stages {
        stage('Checkout') {
            steps {
                 git url: 'https://github.com/Akshay4718/web-jenkins-docker-k8s.git',
                    credentialsId: 'GITHUB_CREDENTIALS',
                    branch: 'main'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                        docker.image(DOCKER_IMAGE).push("latest")
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply the deployment and service configurations
                    sh "kubectl apply -f ~/web-jenkins-docker-k8s/k8s-deployment.yaml"
                    sh "kubectl apply -f ~/web-jenkins-docker-k8s/k8s-service.yaml"
                }
            }
        }
    }
}
