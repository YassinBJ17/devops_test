pipeline { 
    agent any
    triggers {
        githubPush()
    }
    
    stages {
        stage('date') {
            steps {
                sh 'date'
            }
        }
        stage('Build') {
            steps {
                
                git url: 'https://github.com/YassinBJ17/devops_test.git', branch: 'main'
                sh 'mvn clean install'
            }
        }
    }
    
    post {
        failure {
            mail to: 'yassinbj17@gmail.com', 
                 subject: 'Echec de construction Jenkins',
                 body: 'La construction Jenkins a échoué pour le projet ${env.JOB_NAME}. Veuillez vérifier les journaux Jenkins pour plus d\'informations.'
        }
    }
}
