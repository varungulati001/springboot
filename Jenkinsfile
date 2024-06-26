pipeline {
    agent any 
        stages {
            stage ("Checkout From Git") {
                steps {
                    git branch: 'main', url: 'https://github.com/bkrrajmali/springbootapp.git'
                }
            }
        }
    }