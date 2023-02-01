pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '<Github_Credentials_ID>', url: 'https://github.com/<Repository_Name>.git']]])
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t <Image_Name> .'
            }
        }
        stage('Push Docker Image to AWS ECR') {
            steps {
                withAWS(region: '<AWS_Region>', credentials: '<AWS_Credentials_ID>') {
                    sh "aws ecr get-login-password --region <AWS_Region> | docker login --username AWS --password-stdin <AWS_Account_ID>.dkr.ecr.<AWS_Region>.amazonaws.com"
                    sh 'docker push <AWS_Account_ID>.dkr.ecr.<AWS_Region>.amazonaws.com/<Image_Name>'
                }
            }
        }
    }
}

