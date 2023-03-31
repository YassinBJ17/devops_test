pipeline {
  agent any 
  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/YassinBJ17/devops_test.git', branch: 'main'
        echo "Getting project from Git"
        ssh "date"
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
    stage('Run Unit Tests') {
      steps {
        
      }
    } 
  }
post {
    failure {
        script {
            def logContents = currentBuild.rawBuild.getLog(1000)
            mail to: 'yassinbj17@gmail.com',
                 subject: "Build failed for ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                 body: "The build for ${env.JOB_NAME} ${env.BUILD_NUMBER} has failed. Please see the logs below for more information:\n\n${logContents}"
        }
    }
}
}
