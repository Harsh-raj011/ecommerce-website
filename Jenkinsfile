pipeline {
    agent any

    environment {
        IBM_CLOUD_API_KEY = credentials('ibm-cloud-api-key') // Store your IBM Cloud API key in Jenkins credentials
        IBM_CLOUD_REGION = 'us-south' // Change to your region
        APP_NAME = 'app.js' // Change to your app name
        GIT_REPO = 'https://github.com/Harsh-raj011/ecommerce-website.git' // Change to your Git repository
    }

  
        stage('Build') {
            steps {
                script {
                    // Example for a Node.js application
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run your tests here
                    bat 'npm test'
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    // Create a Docker image or package your application
                    bat 'docker build -t ${APP_NAME}:latest .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Login to IBM Cloud
                    bat 'ibmcloud login --apikey ${IBM_CLOUD_API_KEY} --region ${IBM_CLOUD_REGION}'
                    // Push the Docker image to IBM Cloud Container Registry
                    bat 'ibmcloud cr login'
                    bat 'docker tag ${APP_NAME}:latest us.icr.io/your_namespace/${APP_NAME}:latest'
                    bat 'docker push us.icr.io/your_namespace/${APP_NAME}:latest'
                    // Deploy to IBM Cloud (Cloud Foundry or Kubernetes)
                    // For Cloud Foundry
                    bat 'ibmcloud cf push ${APP_NAME} --docker-image us.icr.io/your_namespace/${APP_NAME}:latest'
                    // For Kubernetes, you would use kubectl commands here
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
