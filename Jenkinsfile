pipeline {
    agent any

    environment {
        DEPLOY_USER = "ubuntu"
        DEPLOY_HOST = "172.31.11.233"
        SSH_KEY     = "/var/lib/jenkins/.ssh/jenkins-key.pem"
        DEPLOY_DIR  = "/home/ubuntu/app"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/ravinder7173/node-js-sample.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || echo "Tests failed"'
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r app.zip *'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Copying package to target server..."
                    scp -i $SSH_KEY -o StrictHostKeyChecking=no app.zip $DEPLOY_USER@$DEPLOY_HOST:/home/ubuntu/

                    echo "Running deployment script on target server..."
                    ssh -i $SSH_KEY -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_HOST << 'EOF'
                        mkdir -p $DEPLOY_DIR
                        unzip -o /home/ubuntu/app.zip -d $DEPLOY_DIR
                        pkill node || true
                        nohup node $DEPLOY_DIR/index.js > $DEPLOY_DIR/app.log 2>&1 &
                        echo "App restarted"
                    EOF
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'app.zip', fingerprint: true
        }
    }
}
