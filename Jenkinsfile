pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-demo:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f node-demo || true
                docker run -d -p 3000:3000 --name node-demo node-demo:latest
                '''
            }
        }
    }
}

