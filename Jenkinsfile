pipeline {
    agent {
        label 'ec2-agent'
    }

    environment {
        DEPLOY_USER = 'ubuntu'
        DEPLOY_HOST = '16.170.239.79'
        DEPLOY_PATH = '/home/ubuntu/tomcat9/webapps' 
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Rajveer188/java-app-jenkins.git'
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
                    sh 'scp -o StrictHostKeyChecking=no target/SimpleJavaWebApp.war ubuntu@${DEPLOY_HOST}:${DEPLOY_PATH}'
                }
            }
        }
    }
    post{
        failure{
            emailext (
                    subject: "Build fail: ${env.JOB_NAME} #{env.BUILD_NUMBER}",
                    body: "Check Details - ${env.BUILD_URL}",
                    to: "rjve123009@gmail.com"
                )
        }
    }
}
