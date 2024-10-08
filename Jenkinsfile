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

       stage('Deploy to EC2') {
    steps {
        sshagent (credentials: ['ec2-ssh-credentials']) {
            sh '''
            ssh -o StrictHostKeyChecking=no ubuntu@54.211.23.146 << EOF
            cd /home/ubuntu/simple-node-app
            if git pull origin main; then
                npm install
                pm2 restart all || pm2 start index.js --name "simple-node-app"
            else
                echo "Git pull failed, aborting deployment."
                exit 1
            fi
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
