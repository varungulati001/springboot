pipeline {
    tools {
        maven "Maven3"
    }
    agent any
    
        stages {
            stage ('Checkout from git'){
                steps {
                    git branch: 'main', url: 'https://github.com/bkrrajmali/springbootapp.git'
                }
            }

        }
}
