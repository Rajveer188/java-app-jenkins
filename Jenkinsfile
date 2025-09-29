pipeline {
    agent any

    environment {
        DEPLOY_USER = 'ubuntu'
        DEPLOY_HOST = '13.61.22.218'
        DEPLOY_PATH = '/home/ubuntu/tomcat9/webapps' // or /opt/tomcat9/webapps if using root
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Rajveer188/java-app-jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['ec2-ssh-key-tomcat']) {
                    sh """
                    scp -i rajveer-key.pem target/*.war ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}
                    """
                }
            }
        }
    }
}
