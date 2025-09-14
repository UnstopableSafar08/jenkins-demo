pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/UnstopableSafar08/jenkins-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-static-site .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f static-site || true
                docker run -d --name static-site -p 8081:80 my-static-site
                '''
            }
        }
    }
}
