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

    stage('Start Bisecting') {
      steps {
        script {
          // Using git bisect with a direct command
          bat """
            git bisect start
            git bisect good %GOOD_COMMIT%
            git bisect bad %BAD_COMMIT%
          """
        }
      }
    }

    stage('Locate Bug Commit') {
      steps {
        script {
            bat 'git bisect run powershell -Command "mvn clean test"'
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

