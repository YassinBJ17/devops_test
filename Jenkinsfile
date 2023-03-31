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
        script {
            def logContents = currentBuild.rawBuild.getLog(1000)
            mail to: 'yassinbj17@gmail.com',
                 subject: "Build failed for ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                 body: "The build for ${env.JOB_NAME} ${env.BUILD_NUMBER} has failed. Please see the logs below for more information:\n\n${logContents}"
        } 
    } 
  }
}
