pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = '45.141.151.65/node-demo/node-demo'
        DOCKER_TAG = 'latest'
        CONTAINER_NAME = 'node-demo'
        CONTAINER_PORT = '3000:3000'
    }
    
    triggers {
        githubPush()  // GitHub'a push yapıldığında otomatik tetiklenir
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }
        
        stage('Run Container') {
            steps {
                script {
                    // Eski container varsa durdur ve sil
                    sh """
                        docker rm -f ${CONTAINER_NAME} || true
                        docker run -d -p ${CONTAINER_PORT} --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }
	
	stage('Push to Harbor') {
 	   steps {
        	withCredentials([usernamePassword(
           		credentialsId: 'harbor-credentials',
            		usernameVariable: 'HARBOR_USER',
            		passwordVariable: 'HARBOR_PASS'
        )]) {
            		sh "docker login 45.141.151.65 -u ${HARBOR_USER} -p ${HARBOR_PASS}"
            		sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
        }
    }
}	
    }
    
    post {
        success {
            echo 'Pipeline başarıyla tamamlandı! Container çalışıyor.'
        }
        failure {
            echo 'Pipeline başarısız oldu!'
        }
        always {
            echo 'Pipeline tamamlandı.'
        }
    }
}
