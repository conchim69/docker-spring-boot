pipeline {
    agent any

    environment {
        registry = "590184044177.dkr.ecr.us-east-1.amazonaws.com/my-docker-repo"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/conchim69/docker-spring-boot']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 590184044177.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker push 590184044177.dkr.ecr.us-east-1.amazonaws.com/my-docker-repo:latest"
                    
                }
            }
        }
                
        stage ("Helm install") {
            steps {
		script {
		    sh "helm upgrade first --install --namespace helm-deployment --set image.tag=latest mychart"
		}
            }
        }
    }
}
