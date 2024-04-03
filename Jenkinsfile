pipeline {
  agent any
  tools { 
      maven 'DHT_MVN' 
      jdk 'DHT_SENSE' 
  }
  stages {
    stage('check out') {
      steps {
        git(url: 'https://github.com/spillthebeanss/maven-samples-A6.git', branch: 'master')
      }
    }

    stage('run') {
      steps {
        bat 'mvn verify'
        bat 'mvn test'
        bat 'mvn verify'
        bat 'mvn clean'
      }
    }
  }
}
