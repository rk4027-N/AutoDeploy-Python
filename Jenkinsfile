pipeline {
    agent any

    environment {
        IMAGE_NAME = 'autodeploy-python'
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds'  // Stored in Jenkins credentials
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/rk4027-N/AutoDeploy-Python.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Installing dependencies..."'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running basic test..."'
                sh 'python -c "import app"'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        def image = docker.build("${IMAGE_NAME}:latest")
                        image.push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "docker run -d -p 5000:5000 ${IMAGE_NAME}:latest"
            }
        }
    }
}

