pipeline {
    agent any
    
    environment {
        registry = "686255977433.dkr.ecr.us-east-2.amazonaws.com/cicd"
        imageTag = "${env.BUILD_NUMBER}" // Use Jenkins build number as the tag
    }
  
    stages {
        stage('Cloning Git') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Irfan-2502/K8s.git']])     
            }
        }
        
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:${imageTag}") // Tag with build number
                }
            }
        }

        stage('Pushing to ECR') {
            steps {  
                script {
                    sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin ${registry}'
                    sh "docker push ${registry}:${imageTag}" // Use the variable for the image tag
                }
            }
        }

        stage('K8S Deploy') {
            steps{   
                script {
                    withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
                    sh ('kubectl apply -f  deployments/')
                    }
                }
            }
        }
    }
}
