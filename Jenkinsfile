pipeline {
    agent any

    tools {
        maven 'Maven 3.9.1'
    }

     stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/barnamdas/spring-petclinic.git'
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

       

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/petclinic.yml'
                sh 'kubectl apply -f k8s/db.yml'
            }
        }
    }
}
