pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS_ID = 'azure-service-principal'
        RESOURCE_GROUP = 'rg-jenkins'
        APP_SERVICE_NAME = 'webapijenkinsmuskan982823
'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/MuskanFalwaria/python-webapp.git'
            }
        }

        stage('Set Up Python Environment') {
            steps {
                bat '"C:\\Users\\HP\\AppData\\Local\\Programs\\Python\\Python39\\python.exe" -m venv venv'
                bat '.\\venv\\Scripts\\activate && .\\venv\\Scripts\\python.exe -m pip install --upgrade pip'
                bat '.\\venv\\Scripts\\activate && .\\venv\\Scripts\\python.exe -m pip install -r requirements.txt'
                bat '.\\venv\\Scripts\\activate && .\\venv\\Scripts\\python.exe -m pip install pytest'
            }
        }

        // stage('Run Tests') {
        //     steps {
        //         bat '.\\venv\\Scripts\\activate && .\\venv\\Scripts\\python.exe -m pytest'
        //     }
        // }

        stage('Deploy') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    bat '''
                    if exist publish (rmdir /s /q publish)
                    mkdir publish

                    :: Copy .py files and requirements.txt to publish folder
                    for %%f in (*.py) do copy "%%f" publish\\
                    if exist requirements.txt copy requirements.txt publish\\
                    '''
                    bat 'az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%'
                    bat 'powershell Compress-Archive -Path ./publish/* -DestinationPath ./publish.zip -Force'
                    bat 'az webapp deploy --resource-group %RESOURCE_GROUP% --name %APP_SERVICE_NAME% --src-path ./publish.zip --type zip'
                }
            }
        }
    }

    post {
        failure {
            echo 'Deployment Failed!'
        }
        success {
            echo 'Deployment Successful!'
        }
    }
}


