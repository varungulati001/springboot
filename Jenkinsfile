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
}
