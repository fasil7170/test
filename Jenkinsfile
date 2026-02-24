pipeline {
    agent any

    environment {
        REGISTRY = "docker.io/fazil2664"
        IMAGE_NAME = "jenkins-demo"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t $REGISTRY/$IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Image') {
            steps {
                sh "docker push $REGISTRY/$IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Update GitOps Repo') {
            steps {
                sh """
                git clone https://github.com/YOUR_GITHUB_USERNAME/jenkins-demo-manifests.git
                cd jenkins-demo-manifests
                sed -i 's|image:.*|image: $REGISTRY/$IMAGE_NAME:$IMAGE_TAG|' deployment.yaml
                git config user.email "jenkins@local"
                git config user.name "jenkins"
                git commit -am "Update image to $IMAGE_TAG"
                git push
                """
            }
        }
    }
}
