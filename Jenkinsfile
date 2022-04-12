pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS=credentials('docker-hub-credentials')
    }
    stages {
        stage('Test') {
            agent {
                dockerfile {
                    filename 'Dockerfile.dev'
                    dir 'client'
                    label 'estonezzz/react-test'
                }
            }
            steps {
                sh '''
                    docker run -e CI=true estonezzz/react-test npm test
                '''
            }            
        }
        stage('Build') {
            steps {
                sh '''
                    docker build -t estonezzz/multi-client ./client
                    docker build -t estonezzz/multi-nginx ./nginx
                    docker build -t estonezzz/multi-server ./server
                    docker build -t estonezzz/multi-worker ./worker
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                    docker push estonezzz/multi-client
                    docker push estonezzz/multi-nginx
                    docker push estonezzz/multi-server
                    docker push estonezzz/multi-worker
                '''
            }
        }
    }

}
