def registry  ='https://springbootappncpl.jfrog.io'
pipeline {
    tools {
        maven "Maven3"
    }
    agent any
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
        stage('Checkout From Git') {
            steps {
                git branch: 'main', url: 'https://github.com/bkrrajmali/springbootapp.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Unit Test') {
            steps {
                echo '<----------------------Unit Test Under Progess-------------------->'
                sh 'mvn surefire-report:report'
                echo '<----------------------Unit Test Finished------------------------->'
            }
        }
        stage('SonarQube Analysis') {
            steps {
               script {
                withSonarQubeEnv('sonar-server') {
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=springbootapp -Dsonar.projectKey=bkrrajmali_springbootapp '''
                }
               }
            }
        }
        stage ('Quality Gate'){
        steps {
            script {
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
            }
        }
      }
    }
}
