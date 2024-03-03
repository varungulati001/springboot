pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo"
	    SCANNER_HOME= tool 'sonar-scanner'
    }
   stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/bkrrajmali/springbootapp.git'
            }
        }

      stage ('Build') {
          steps {
            sh 'mvn clean install'           
            }
      }
   stage('Unit Test') {
      steps {
        echo '<--------------- Unit Testing started  --------------->'
        sh 'mvn surefire-report:report'
        echo '<------------- Unit Testing stopped  --------------->'
      }
    }

   stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=springboot \
                    -Dsonar.projectKey=springboot'''
                }
            }
        }
        stage("quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
    //Building Docker images
    stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t myrepo .'
                }
            }
    }


stage('Upload to Nexus') {
    steps {
        script {
            nexusArtifactUploader artifacts: [
                // Define the artifacts you want to upload to Nexus
                [artifactId: 'springbootApp', file: 'Springbootapp/target/springbootApp.jar', type: 'jar']
                // Add more artifacts if needed
            ],
            credentialsId: 'nexus',
            groupId: 'com.tcs.angularjs',
            nexusUrl: '18.206.212.197:8081',
            nexusVersion: '3', // Adjust this to your Nexus version (2 or 3)
            protocol: 'http', // Use 'https' if your Nexus server uses HTTPS
            repository: 'maven3',
            version: '1.0.0' // Replace with your desired version
        }
    }
}
  //  stage('Upload to Nexus') {
  //     steps {
  //       script {
  //          echo '<--------------- War Publish Started --------------->'
  //         nexusArtifactUploader artifacts: [
  //           [artifactId: 'springbootApp', classifier: '', file: 'Springbootapp/target/springbootApp.jar', type: 'jar']
  //           ], credentialsId: 'nexus', groupId: 'com.tcs.angularjs', nexusUrl: '18.206.212.197:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven3', version: '1.0'
  //           echo '<--------------- War Publish Ended --------------->'  
          
  //       }
  //     }
	// }
    // // Uploading Docker images into AWS ECR
    // stage('Pushing to ECR') {
    //  steps{  
    //      script {
    //             sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 923770093922.dkr.ecr.us-east-1.amazonaws.com'
    //             sh 'docker tag myrepo:latest 923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
    //             sh 'docker push 923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
    //      }
    //     }
    //   }

    //    stage('Deploy to EKS') {
    //         steps {
    //             script {
    //                 // Authenticate with the EKS cluster (ensure AWS credentials are configured)
    //                 sh 'aws eks --region us-east-1 update-kubeconfig --name demo-eks'
                    
    //                 // Apply Kubernetes manifest files to deploy your application
    //                   sh "kubectl delete -f eks-deploy-k8s.yaml"
    //                   sh "kubectl apply -f eks-deploy-k8s.yaml"
    //             }
    //         }
    //     }
       
    }
}
