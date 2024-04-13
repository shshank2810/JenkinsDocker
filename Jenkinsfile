pipeline {
 
agent any
 
environment {
 
    IMAGE_NAME='shshank28/jenkinsdemo'
 
    IMAGE_TAG='latest'
 
}
stages {
 
stage('Checkout') {
 
steps {
 
// Checkout your code from your Git repository
 
git 'https://github.com/shshank2810/JenkinsDocker.git'
 
}
 
}
 
stage('Restore Packages') {
 
steps {
 
// Restore NuGet packages
 
sh 'dotnet restore'
 
}
 
}
 
stage('Build') {
 
steps {
 
// Build the .NET project using MSBuild
 
sh 'dotnet build'
 
}
 
}

stage('SonarQube Analysis') {

            environment {

                scannerHome = tool 'SonarScanner for MSBuild'

            }

            steps {

                script {

                    withSonarQubeEnv('SonarQube-Server') {

                        sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll begin /k:\"Devops\""

                        sh "dotnet build"

                        sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll end"

                    }

                }

            }

        }
 
stage('Test') {
 
steps {
 
// Build the .NET project using MSBuild
 
sh 'dotnet test'
 
}
 
}
 
stage('Docker Build'){
    steps{
        script{
            docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
        }
    }
}
 
 
stage('Docker Push'){
 
    steps{
 
        script{
 
            docker.withRegistry('https://index.docker.io/v1/','docker_login'){

 
                docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
 
            }
        }
 
    }
 
}
stage('Run container') {
 
steps {
 
script{
    sh 'docker rm -f devops || true '
    sh "docker run -d --name devops -p 8000:80 ${IMAGE_NAME}:${IMAGE_TAG}"
}
 
}
 
}
}
 
}
