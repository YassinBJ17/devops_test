pipeline {
  agent any
  environment {
    SONAR_LOGIN = credentials('mon-sonarqube-login') // Créer un secret Jenkins pour stocker votre jeton d'authentification SonarQube
  }
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
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') { // Créer une configuration de serveur SonarQube nommée "SonarQube" dans la section "Gestion de configurations" de Jenkins
          sh 'mvn sonar:sonar'
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
