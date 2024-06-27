pipeline {
    tools {
        maven 'Maven3'
    }
    agent any 
    stages {
        stage ('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/bkrrajmali/springbootapp.git'
            }
        }
        stage ('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
