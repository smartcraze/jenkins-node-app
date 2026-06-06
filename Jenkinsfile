pipeline {
    agent any

    environment {
        IMAGE_NAME = "smartcraze/jenkins-node-app"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker run -p 3000:3000 --rm  -d $IMAGE_NAME'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}