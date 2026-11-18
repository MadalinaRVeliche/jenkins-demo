pipeline {
    agent any

    parameters {
        booleanParam(name: 'DEPLOY_PROD', defaultValue: false,
                     description: 'Bifează pentru deploy în producție')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Compilez aplicația...'
            }
        }

        stage('Test') {
            parallel {
                stage('Unit') {
                    steps {
                        echo 'Rulez testele unitare...'
                        sh 'sleep 5'
                    }
                }
                stage('Integration') {
                    steps {
                        echo 'Rulez testele de integrare...'
                        sh 'sleep 5'
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                expression { params.DEPLOY_PROD }
            }
            input {
                message 'Deploy în producție?'
                ok 'Da, livrează'
            }
            steps {
                withCredentials([string(credentialsId: 'parola-server', variable: 'PAROLA')]) {
                    sh 'echo "Mă conectez la server cu parola: $PAROLA"'
                }
                echo 'Aplicație livrată!'
            }
        }
    }

    post {
        success { echo 'Pipeline reușit!' }
        failure { echo 'Pipeline eșuat!' }
        always  { echo 'Curăț workspace-ul' }
    }
}
