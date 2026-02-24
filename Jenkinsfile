pipeline {
    agent {
        docker { image 'maven:3.9.4-openjdk-17' }
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/fasil7170/test.git', branch: 'main'
            }
        }
        stage('Build') {
            steps { sh 'mvn clean compile' }
        }
        stage('Test') {
            steps { sh 'mvn test' }
        }
    }
    post {
        success { echo 'Pipeline completed successfully! ✅' }
        failure { echo 'Pipeline failed! ❌' }
    }
}
