pipeline {
  agent any
  tools { 
      maven 'DHT_MVN' 
      jdk 'DHT_SENSE' 
  }
  stages {
    stage('checkout') {
      steps {
        git(url: 'https://github.com/spillthebeanss/maven-samples-A6.git', branch: 'master')
      }
    }

    stage('Find Bug-Introducing Commit') {
        steps {
            script {
                // Initialize environment variables for the known good and bad commits
                env.GOOD_COMMIT = '98ac319c0cff47b4d39a1a7b61b4e195cfa231e5'
                env.BAD_COMMIT = '198644632661c67b6c32f59e9047c11a70685e15'

                // Bisect process script
                bat '''
                git bisect start
                git bisect bad $BAD_COMMIT
                git bisect good $GOOD_COMMIT
                
                git bisect run bat -c 'mvn clean test'
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

