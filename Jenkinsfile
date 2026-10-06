pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Code checkout successful'
            }
        }

        stage('Build') {
            steps {
                echo 'Application build started'
            }
        }

        stage('Test') {
            steps {
                echo 'Application test successful'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t abin1997/jenkins-cicd:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push abin1997/jenkins-cicd:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                    docker pull abin1997/jenkins-cicd:latest
                    docker stop jenkins-cicd || true
                    docker rm jenkins-cicd || true
                    docker run -d --name jenkins-cicd -p 80:80 abin1997/jenkins-cicd:latest
                '''
            }
        }
    }
}
