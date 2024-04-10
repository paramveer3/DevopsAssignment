pipeline {
    agent any

    environment {
        IMAGE_NAME = 'paramveer03/devopsdotnet'
	CONTAINER_NAME = 'devopsdotnetcont'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build dotnetapplication/dotnetapplication.csproj --configuration Release'
            }
        }

      

        stage('Docker Build') {
            steps {
                // Build your Docker image
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    // Login to Docker Hub (or your Docker registry)
                    // Make sure to set your credentials in Jenkins credential store
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                        // Push your Docker image
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Stop the currently running container (if any)
                    sh 'docker rm -f ${CONTAINER_NAME} || true'
                    // Run the new container
                    sh "docker run -d --name ${CONTAINER_NAME} -p 8885:80 ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}
