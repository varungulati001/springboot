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
  stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}
