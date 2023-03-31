pipeline { 
  agent any      
  stages {    
    stage('GIT') { 
      steps { 
        git url: 'https://github.com/YassinBJ17/devops_test.git', branch: 'main'
        echo "Getting project from Git"
        sh "date"
      }
    }
    stage('MVN Clean') {
      steps {
        sh 'mvn clean'
      }
    }
    stage('MVN Compile') {
      steps {
        sh 'mvn compile'
      }
    }
    stage('Tests') {
      steps {
          echo 'mvn test'
      }
    } 
    
     stage('MVN Clean install') {
      steps {
        sh 'mvn package -DskipTests=true'
      }
    }
    
     stage('MVN SONARQUEBE') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    
    
    
    stage('NEXUS'){
      steps{
        script{
          
          nexusArtifactUploader artifacts: 
            [
              [artifactId: 'tpAchatProject',
               classifier: '',
               file: 'target//tpAchatProject-1.0.jar',
               type: 'jar'
              ]
            ], 
            credentialsId: 'jenkins-nexus',
            groupId: 'com.esprit.examen',
            nexusUrl: '192.168.33.10:8081',
            nexusVersion: 'nexus2',
            protocol: 'http',
            repository: 'jenkins_nexus',
            version: '1.0'
          
        }
      }
    }    
          
  }
 post {
        failure {
            mail to: 'emna.bentijani@esprit.tn',
            subject: 'Build failed',
            body: 'The build has failed. Please check Jenkins for details.'
        }
    }
}
