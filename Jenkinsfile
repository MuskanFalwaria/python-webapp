pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS_ID = 'azure-service-principal' // Replace with your actual credentials ID in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/MuskanFalwaria/python-webapp.git', branch: 'main'
            }
        }

        stage('Azure Login') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: "${AZURE_CREDENTIALS_ID}",
                                                      subscriptionIdVariable: 'AZURE_SUBSCRIPTION_ID',
                                                      clientIdVariable: 'AZURE_CLIENT_ID',
                                                      clientSecretVariable: 'AZURE_CLIENT_SECRET',
                                                      tenantIdVariable: 'AZURE_TENANT_ID')]) {
                    sh '''
                        echo Logging into Azure...
                        az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID
                        az account set --subscription $AZURE_SUBSCRIPTION_ID
                        echo Azure login successful
                    '''
                }
            }
        }

        stage('Run Application Script') {
            steps {
                sh '''
                    echo "Running your app logic here..."
                    # Add your deployment or testing script here
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
