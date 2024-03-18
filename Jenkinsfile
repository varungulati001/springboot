pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo"
	    SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
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
 stage("Sonarqube Analysis "){
            steps{
 		 withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=springbootapp \
                    -Dsonar.projectKey=springbootapp '''
                }
	    }
 }
        stage("quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                }
            }
        }
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
           echo '<--------------- War Publish Started --------------->'
          nexusArtifactUploader artifacts: [
            [artifactId: 'springbootApp', classifier: '', file: 'Springbootapp/target/springbootApp.jar', type: 'jar']
            ], credentialsId: 'nexus', groupId: 'org.springframework.boot', nexusUrl: '18.206.212.197:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven', version: '1.0'
            echo '<--------------- War Publish Ended --------------->'  
          
        }
      }
	}
    }
}
