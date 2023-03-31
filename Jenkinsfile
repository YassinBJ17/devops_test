pipeline { 
  agent any   
  
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.33.10:8081"
        NEXUS_REPOSITORY = "jenkins_nexus"
        NEXUS_CREDENTIAL_ID = "jenkins_nexus"
    }
  
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
    
    
    
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        
    }
}


 post {
        failure {
            mail to: 'yassinbj17@gmail.com',
            subject: 'Build failed',
            body: 'The build has failed. Please check Jenkins for details.'
        }
    }
}
