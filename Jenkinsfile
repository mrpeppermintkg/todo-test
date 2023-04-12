pipeline {
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['DEV', 'STAGE', 'PROD'], description: 'Choose the environment to deploy the app to')
    }
    stages {
        stage('Checkout') {
            steps {
                node(params.ENVIRONMENT) {
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                node(params.ENVIRONMENT) {
                    sh 'pip install -r requirements.txt'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def checkoutDir = pwd()
                    echo "The code is checked out in ${checkoutDir}"
                }
                node(params.ENVIRONMENT) {
                    checkout scm
                    sh "cd ${env.WORKSPACE} && pip install -r requirements.txt"
                }
            }
        }
    }
}
