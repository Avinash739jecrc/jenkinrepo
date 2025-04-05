pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS_ID = 'azure-service-principal'
        RESOURCE_GROUP = 'rg-jenkins'
        APP_SERVICE_NAME = 'webapijenkinsavinash'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // ✅ FIXED: Use correct branch name
                git branch: 'master', url: 'https://github.com/Avinash739jecrc/jenkinrepo'
            }
        }

        stage('Build .NET App') {
            steps {
                bat 'dotnet restore'
                bat 'dotnet build --configuration Release'
                bat 'dotnet publish --configuration Release --output publish'
            }
        }

        stage('Zip Artifact') {
            steps {
                // ✅ FIXED: Ensure contents are zipped correctly for Azure
                bat 'powershell -Command "Compress-Archive -Path publish\\* -DestinationPath publish.zip -Force"'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    // ✅ FIXED: Use %VAR% for batch scripts
                    bat '''
                        az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%
                        az webapp deploy --resource-group %RESOURCE_GROUP% --name %APP_SERVICE_NAME% --src-path publish.zip --type zip
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo ' Deployment Failed!'
        }
    }
}
