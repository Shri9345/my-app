pipeline {
    agent any

    environment {
        IMAGE_NAME = "shri9345/my-app"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // Replace with your credential ID
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Shri9345/my-app.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            steps {
                echo "Running container to test app..."
                sh 'docker run --rm $IMAGE_NAME'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "Pushing Docker image to DockerHub..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                // Add deployment commands here, e.g., docker run or kubectl apply
            }
        }
    }

    post {
        always {
            echo "Cleaning up..."
            sh 'docker system prune -f'
        }
        failure {
            echo "Pipeline failed!"
        }
        success {
            echo "Pipeline completed successfully!"
        }
    }
}
