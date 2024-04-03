pipeline {
  agent any
  tools { 
      maven 'DHT_MVN' 
      jdk 'DHT_SENSE' 
  }
  environment {
      // Define environment variables for Jenkins to use in the pipeline
      // Initialize environment variables for the known good and bad commits
      GOOD_COMMIT = '98ac319c0cff47b4d39a1a7b61b4e195cfa231e5'
      BAD_COMMIT = '198644632661c67b6c32f59e9047c11a70685e15'
  }
  stages {
    stage('checkout') {
      steps {
        git(url: 'https://github.com/spillthebeanss/maven-samples-A6.git', branch: 'master')
      }
    }

    stage('Locate Bug Commit') {
        steps {
            script {
                // Bisect process script
                bat '''
                  git bisect start
                  git bisect bad %BAD_COMMIT%
                  git bisect good %GOOD_COMMIT%
                  
                  git bisect run cmd /c "
                    mvn clean test
                    if errorlevel 1 (
                        exit /b 1  ; mark this commit as bad
                    ) else (
                        exit /b 0  ; mark this commit as good
                    )
                    "
                '''
            }
        }
    }
    
  }
  post {
      always {
          script {
              // Cleanup: make sure to end the bisect session after the job, even if it fails
              bat 'git bisect reset'
          }
      }
  }

}

