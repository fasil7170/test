pipeline {
    agent {
        kubernetes {
            label 'jenkins-maven'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.9.4-jdk-17   # <- fixed valid tag
    tty: true
    command:
    - cat
"""
        }
    }

    stages {
        stage('Build') {
            steps {
                container('maven') {
                    sh 'mvn clean install'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}
