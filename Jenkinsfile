pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/anilgowda28/jenkins-demo.git'
            }
        }

        stage('Build with Maven') {
            steps {
                dir('java-app') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                dir('java-app') {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }
}
