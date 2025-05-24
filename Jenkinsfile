pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/ravinder7173/node-js-sample.git'
            }
        }

        stage('Install') {
            steps {
                // Use './mvnw install' if it's a Java project
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Use './mvnw test' if it's a Java project
                sh 'npm test'
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r app.zip .'
            }
        }

        stage('Deploy') {
            steps {
                // Replace <target-ip> with your actual target EC2 instance IP
                sh 'scp -i ~/.ssh/id_rsa app.zip ubuntu@172.31.11.233:/home/ubuntu/'
                sh 'ssh -i ~/.ssh/id_rsa ubuntu@172.31.11.233 "bash deploy.sh"'
            }
        }
    }
}
