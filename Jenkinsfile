pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Cloning the repository
                git url: 'https://github.com/arielaviv/Store.git', branch: 'main'
            }
        }
        
        stage('Build') {
            steps {
                // Commands for building the project
                echo 'Building the project...'
                // Add build steps here, e.g., running shell commands or scripts
                // sh 'mvn clean install' or 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                // Commands for testing the project
                echo 'Running tests...'
                // Add testing commands here, e.g., for unit or integration testing
                // sh 'mvn test' or 'npm test'
            }
        }
        
        stage('Deploy') {
            steps {
                // Commands for deployment
                echo 'Deploying the project...'
                // Add deployment commands, e.g., deploying to a server, Kubernetes, etc.
            }
        }
    }

    post {
        always {
            // Archive build results, clean workspace, notify, etc.
            echo 'Pipeline finished.'
        }
    }
}
