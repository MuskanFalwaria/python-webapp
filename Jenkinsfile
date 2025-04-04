pipeline {
    agent any
    environment {
        AZURE_CREDENTIALS = credentials('azure-service-principal')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main' , url:  'https://github.com/MuskanFalwaria/python-webapp.git'
            }
        }
        stage('Build') {
            steps {
                bat 'echo Hello from Windows'
            }
        }
        stage('Publish') {
            steps {
                bat 'powershell -Command "Compress-Archive -Path * -DestinationPath app.zip -Force"'
            }
        }
        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal('azure-service-principal')]) {
                    bat '''
                        az login --service-principal -u %AZURE_CREDENTIALS_USR% -p %AZURE_CREDENTIALS_PSW% --tenant %AZURE_CREDENTIALS_TEN%
                        az webapp up --name myPythonApp --resource-group myResourceGroup --runtime "PYTHON:3.9" --src-path .
                    '''
                }
            }
        }
    }
}
