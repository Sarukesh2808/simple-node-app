node {
    stage('Checkout') {
        git url: 'https://github.com/Sarukesh2808/simple-node-app.git', branch: 'main'
    }
    stage('Install Dependencies') {
        sh 'npm install'
    }
    stage('Run Tests') {
        sh 'npm test'
    }
    stage('Build') {
        sh 'npm run build'
    }
    stage('Deploy') {
        echo 'Deploying to EC2...'
        sh '''
        ssh -o StrictHostKeyChecking=no ubuntu@<EC2-IP-Address> << EOF
        cd /home/ubuntu/simple-node-app
        git pull origin main
        npm install
        pm2 restart all || pm2 start index.js --name "simple-node-app"
        EOF
        '''
    }
}
