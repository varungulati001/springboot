pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/writetoritika/springboot-app']]])     
            }
        }
      stage ('Build') {
          steps {
            sh 'mvn clean install'           
            }
      }
    // Building Docker images
    stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t myrepo .'
                }
            }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 923770093922.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker tag myrepo:latest 923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
                sh 'docker push 923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
         }
        }
      }

       stage('Deploy to EKS') {
            steps {
                script {
                    // Authenticate with the EKS cluster (ensure AWS credentials are configured)
                    sh 'aws eks --region us-east-1 update-kubeconfig --name demo-eks'
                    
                    // Apply Kubernetes manifest files to deploy your application
                    sh 'kubectl apply -f path/to/your/kubernetes-manifests/'
                }
            }
        }
       }
    }
}
