pipeline {
    agent any
    environment {
        IMAGE_NAME = "smartcraze/jenkins-node-app"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Deploy Container') {
            steps {

                sh '''
                    docker rm -f nodeapp || true

                    docker run -d \
                      --name nodeapp \
                      -p 3000:3000 \
                      $IMAGE_NAME
                '''
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