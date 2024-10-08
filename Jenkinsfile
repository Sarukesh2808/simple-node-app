pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Sarukesh2808/simple-node-app.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent (credentials: ['ec2-ssh-credentials']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@<EC2-IP-Address> << EOF
                    cd /path-to-your-app-directory/
                    git pull origin main
                    npm install
                    pm2 restart all || pm2 start index.js --name "simple-node-app"
                    EOF
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
