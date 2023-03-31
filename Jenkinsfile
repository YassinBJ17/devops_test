pipeline {
  agent any
 
  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/YassinBJ17/devops_test.git', branch: 'main'
      }
    }
    stage('Maven Clean') {
      steps {
        sh 'mvn clean'
      }
    }
    stage('Maven Compile') {
      steps {
        sh 'mvn compile'
      }
    }
   
    }
    stage('Run Unit Tests') {
      steps {
        sh 'mvn test'
      }
    } 
  }
  post {
    failure {
      mail to: 'yassinbj17@gmail.com',
           subject: "Build failed for ${env.JOB_NAME} ${env.BUILD_NUMBER}",
           body: "The build for ${env.JOB_NAME} ${env.BUILD_NUMBER} has failed. Please check the logs for more information."
    }
  }
}
