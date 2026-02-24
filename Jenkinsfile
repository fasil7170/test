pipeline {
    agent {
        kubernetes {
            label 'jenkins-maven-full'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: jnlp
    image: jenkins/inbound-agent:latest   # Jenkins agent container
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
    tty: true
  - name: maven
    image: maven:3.9.4-jdk-17              # Correct Maven image
    command:
    - cat
    tty: true
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
