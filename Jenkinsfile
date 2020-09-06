pipeline {
    agent { label "master" }
    environment {
        ECR_REGISTRY = "088674315177.dkr.ecr.eu-west-3.amazonaws.com"
        APP_REPO_NAME= "clarusway-repo/to-do-app"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build --force-rm -t "$ECR_REGISTRY/$APP_REPO_NAME:latest" .'
                sh 'docker image ls'
            }
        }
        stage('Push Image to ECR Repo') {
            steps {
                sh 'aws ecr get-login-password --region eu-west-3 | docker login --username AWS --password-stdin  "$ECR_REGISTRY"'
                sh 'docker push "$ECR_REGISTRY/$APP_REPO_NAME:latest"'
            }
        }
    }
    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}
