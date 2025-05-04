pipeline {
    agent any

    tools {
        maven 'Maven 3.9.1'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/barnamdas/spring-petclinic'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t petclinic-app .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASSWORD')]) {
                    sh """
                    echo "$DOCKER_PASSWORD" | docker login -u yourdockerhubusername --password-stdin
                    docker tag petclinic-app yourdockerhubusername/petclinic-app:latest
                    docker push yourdockerhubusername/petclinic-app:latest
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/petclinic.yml'
                sh 'kubectl apply -f k8s/db.yml'
            }
        }
    }
}
