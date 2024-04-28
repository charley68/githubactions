
pipeline {
    agent any

    environment {
        AWS_CLUSTER_NAME = "demo-cluster"
        AWS_ACCOUNT_ID = "868171460502"
        AWS_DEFAULT_REGION = "eu-west-2"
        IMAGE_REPO_NAME = "steve-repo"
        IMAGE_TAG = "latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }

    stages {
     stage('Cloning Git') {
            steps {
                checkout scm
            }
        }

        stage('Logging into AWS ECR') {
            steps {
                script {
                sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                }
                 
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

        stage('Pushing to ECR') {
         steps{  
            script {
                    sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
                    sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
            }
            }
        }

        stage('Deploy'){
            steps {
                    sh 'aws eks update-kubeconfig  --name  ${AWS_CLUSTER_NAME}  --region ${AWS_DEFAULT_REGION}'
                    sh 'sed -i.bak "s|DOCKER_IMAGE|${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:$IMAGE_TAG|g" manifests/deployment.yml'
                    sh 'kubectl apply -f manifests/deployment.yml'
                    sh 'kubectl apply -f manifests/service.yml'
            }
     }
    }
}
