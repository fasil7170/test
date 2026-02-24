pipeline {
    agent {
        // Use a Maven image with Java 17
        docker { image 'maven:3.9.4-openjdk-17' }
    }
    stages {
        stage('Checkout') {
            steps {
                // Pull code from your GitHub repo
                git url: 'https://github.com/fasil7170/test.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                // Compile the project
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh 'mvn test'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully! ✅'
        }
        failure {
            echo 'Pipeline failed! ❌'
        }
    }
}
