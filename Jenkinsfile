pipeline {
    agent { label 'docker-agent' } // Use your Docker-enabled agent

    environment {
        REGISTRY = "docker.io/fazil2664"
        IMAGE_NAME = "jenkins-demo"
        IMAGE_TAG = "${BUILD_NUMBER}"
        GITOPS_REPO = "https://github.com/YOUR_GITHUB_USERNAME/jenkins-demo-manifests.git"
        GITOPS_BRANCH = "main"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $REGISTRY/$IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds', // Your Docker Hub credentials in Jenkins
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $REGISTRY/$IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Update GitOps Repository') {
            steps {
                dir('gitops') {
                    sh """
                        git clone -b $GITOPS_BRANCH $GITOPS_REPO .
                        sed -i 's|image:.*|image: $REGISTRY/$IMAGE_NAME:$IMAGE_TAG|' deployment.yaml
                        git config user.email "jenkins@local"
                        git config user.name "jenkins"
                        git commit -am "Update image to $IMAGE_TAG"
                        git push origin $GITOPS_BRANCH
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after build
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}
