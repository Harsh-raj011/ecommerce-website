pipeline {
    agent any

    environment {
        IBM_CLOUD_API_KEY = credentials('ibm-cloud-api-key') // Ensure this matches your Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git 'https://github.com/yourusername/your-repo.git' // Replace with your repository URL
            }
        }

        stage('Build') {
            steps {
                // Install dependencies and build the application
                script {
                    sh 'npm install'
                    sh 'npm run build' // Adjust this command based on your build process
                }
            }
        }

        stage('Test') {
            steps {
                // Run tests
                script {
                    sh 'npm test'
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deploy to IBM Cloud
                script {
                    sh '''
                    curl -sL https://ibm.biz/idt-installer | bash
                    ibmcloud login --apikey $IBM_CLOUD_API_KEY -r us-south
                    ibmcloud cf push my-ecommerce-app -p ./build # Adjust the path as necessary
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
