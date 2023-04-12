pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['DEV', 'STAGE', 'PROD'], description: 'Choose the environment to deploy the app to')
    }
    stages {
        stage('Checkout') {
            agent {
                label params.ENVIRONMENT
            }
            steps {
                checkout scm
            }
        }
        stage('Build') {
            agent {
                label params.ENVIRONMENT
            }
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Deploy') {
            agent {
                label params.ENVIRONMENT
            }
            steps {
                script {
                    def checkoutDir = pwd()
                    echo "The code is checked out in ${checkoutDir}"
                }
                checkout scm
                sh "cd ${env.WORKSPACE} && pip install -r requirements.txt"
            }
        }
    }
}
