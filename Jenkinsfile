pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pull code from GitHub
                git branch: 'main', url: 'https://github.com/YOUR_GITHUB_USERNAME/jenkins-demo.git'
            }
        }

        stage('Build & Test with Maven') {
            steps {
                dir('java-app') {
                    // Clean old build, run tests, create WAR
                    sh 'mvn clean test package'
                }
            }
        }

        stage('Archive WAR') {
            steps {
                dir('java-app') {
                    archiveArtifacts artifacts: 'target/*.war', fingerprint: true
                }
            }
        }

        stage('Deploy to Remote Tomcat') {
            steps {
                // Use SSH credentials configured in Jenkins (ID: tomcat-ssh-key)
                sshagent(credentials: ['tomcat-ssh-key']) {
                    sh '''
                        echo "Starting deployment to remote Tomcat..."

                        # Paths and config
                        WAR_FILE=java-app/target/java-app.war
                        REMOTE_USER=REMOTE_SSH_USER
                        REMOTE_HOST=REMOTE_TOMCAT_IP
                        REMOTE_TOMCAT_WEBAPPS=/opt/tomcat/webapps
                        REMOTE_TOMCAT_SERVICE=tomcat

                        echo "Copying WAR to remote server..."
                        scp -o StrictHostKeyChecking=no "$WAR_FILE" ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_TOMCAT_WEBAPPS}/java-app.war

                        echo "Restarting remote Tomcat..."
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} "sudo systemctl restart ${REMOTE_TOMCAT_SERVICE}"

                        echo "Deployment completed."
                    '''
                }
            }
        }
    }
}

