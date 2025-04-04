pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS_ID = 'azure-service-principal'
        RESOURCE_GROUP = 'rg-jenkins'
        APP_SERVICE_NAME = 'pythonwebapp982823'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/MuskanFalwaria/python-webapp.git'
            }
        }

        stage('Build') {
            steps {
                bat 'python -m venv venv'
                bat '.\\venv\\Scripts\\activate && pip install -r requirements.txt'
            }
        }

        stage('Package') {
            steps {
                bat 'powershell -Command "Compress-Archive -Path * -DestinationPath app.zip -Force"'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    bat 'az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%'
                    bat 'az webapp deploy --resource-group %RESOURCE_GROUP% --name %APP_SERVICE_NAME% --src-path app.zip --type zip'
                }
            }
        }
    }

    post {
        success {
            echo 'Python Web App Deployed Successfully!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
