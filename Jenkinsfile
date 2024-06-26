pipeline {
    tools{
        maven "Maven3"
    }
    agent any 
        stages {
            stage ("Checkout From Git") {
                steps {
                    git branch: 'main', url: 'https://github.com/bkrrajmali/springbootapp.git'
                }
            }
            stage ("Maven Build") {
                steps {
                    sh 'mvn clean install'
                }
            }
            stage ('Unit Test'){
                steps {
                    echo '<----------------------Unit Test Under Progess ------------------>'
                    sh 'mvn surefire-report:report'
                    echo '<----------------------Unit Test Done------------------>'
                }
            }
            stage ('Sonarqube analysis'){
            steps {
                script {
                    withSonarQubeEnv('sonar') {
                        sh  ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=springbootapp \
                        -Dsonar.projectKey=springbootapp '''
                    }
                }
            }
        }

        }
    }
