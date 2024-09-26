pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Use the credentials stored in Jenkins
        DOCKERHUB_REPO = 'arielaviv/my-node-app'
        MONGO_URI = "mongodb+srv://<mongo>:<mongo>@cluster0.fo8vb.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git url: 'https://github.com/arielaviv/Store.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in the repo
                    def app = docker.build("${env.DOCKERHUB_REPO}:latest")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKERHUB_CREDENTIALS) {
                        echo 'Logged in to Docker Hub'
                    }
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    def app = docker.image("${env.DOCKERHUB_REPO}:latest")
                    app.push()
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply the Kubernetes deployment and service YAML files
                    sh 'kubectl apply -f C:/Users/ariel/Desktop/DevOps/Store/k8s/nodejs-deployment.yml'
                    sh 'kubectl apply -f C:/Users/ariel/Desktop/DevOps/Store/k8s/nodejs-service.yml'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    // Check if the deployment succeeded by verifying if pods are running
                    def deploymentStatus = sh(script: 'kubectl get pods --selector=app=nodejs-app --field-selector=status.phase!=Running', returnStatus: true)
                    if (deploymentStatus != 0) {
                        error "Deployment verification failed! Rolling back..."
                    }
                }
            }
        }

        stage('Rollback') {
            when {
                failure() // This stage will only run if there is a failure in the previous stages
            }
            steps {
                script {
                    // Rollback the Kubernetes deployment to the previous version
                    sh 'kubectl rollout undo deployment/nodejs-app'
                    echo "Rolled back to the previous version."
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
