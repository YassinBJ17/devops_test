pipeline { 
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/YassinBJ17/devops_test.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
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
