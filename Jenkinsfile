pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "heyitsarsh/college-department-portal"
        KUBECONFIG = "C:\\Users\\lenovo\\.kube\\config"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/HeyItsArsh/Agile-Lab-8-3.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:%BUILD_NUMBER% .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat 'docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'
                    bat 'docker push %DOCKER_IMAGE%:%BUILD_NUMBER%'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl config current-context'
                bat 'kubectl get nodes'
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl set image deployment/college-department-portal college-department-portal=%DOCKER_IMAGE%:%BUILD_NUMBER%'
                bat 'kubectl rollout status deployment/college-department-portal'
            }
        }
    }
}
