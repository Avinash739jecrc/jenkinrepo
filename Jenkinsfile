pipeline {
 agent any
 environment {
 AZURE_CREDENTIALS = credentials('azure-service-principal')
 }
 stages {
 stage('Checkout') {
 steps {

 git 'https://github.com/Avinash739jecrc/jenkinrepo'
 }
 }
 stage('Build') {
 steps {
 script {
 bat 'dotnet restore'
 bat 'dotnet build --configuration Release'
 }
 }
 }
 stage('Publish') {
 steps {
 bat 'dotnet publish -c Release -o publish/'
 }
 }
 stage('Deploy to Azure') {
 steps {
 withCredentials([azureServicePrincipal('azure-service-principal')]) {
 bat 'az login --service-principal -u $AZURE_CREDENTIALS_USR -p'
'$AZURE_CREDENTIALS_PSW --tenant $AZURE_CREDENTIALS_TEN'
 bat 'az group create --name myResourceGroup --location eastus'
 bat 'az webapp create --name myAppService --resource-group'
'myResourceGroup --plan myAppPlan --runtime "DOTNETCORE:8.0"'
 bat 'az webapp deploy --name myAppService --resource-group 
myResourceGroup --src-path publish/'
 }
 }
 }
 }
