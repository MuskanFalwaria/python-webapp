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
        withCredentials([
            string(credentialsId: 'AZURE_CLIENT_ID', variable: 'AZURE_CLIENT_ID'),
            string(credentialsId: 'AZURE_CLIENT_SECRET', variable: 'AZURE_CLIENT_SECRET'),
            string(credentialsId: 'AZURE_TENANT_ID', variable: 'AZURE_TENANT_ID'),
            string(credentialsId: 'AZURE_SUBSCRIPTION_ID', variable: 'AZURE_SUBSCRIPTION_ID')
        ]) {
            bat """
                echo Logging into Azure...
                az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%
                echo Setting subscription...
                az account set --subscription %AZURE_SUBSCRIPTION_ID%
                echo Deployment script would go here.
            """
        }
    }
}

    }
}
