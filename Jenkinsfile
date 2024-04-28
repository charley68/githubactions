
pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = "your account id"
        AWS_DEFAULT_REGION = "eu-west-2"
        IMAGE_REPO_NAME = "steve"
        IMAGE_TAG = "latest"
        REPOSITORY_URI = "your repository url"
    }

    stages {
     stage('Cloning Git') {
            steps {
                checkout scm
                //checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'your github url']]])     
            }
        }

        // Building Docker images
        stage('Building image') {
            steps {
                script {
                    sh """ docker build -t ${IMAGE_REPO_NAME}:${IMAGE_TAG} ."""
                }
            }
        }


        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
