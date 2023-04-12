pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['DEV', 'STAGE', 'PROD'], description: 'Choose the environment to deploy the app to')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt'
                
            }
        }
        stage('Deploy') {
            steps {
                checkout scm
                script {
                    def checkoutDir = pwd()
                    echo "The code is checked out in ${checkoutDir}"
                }
                script {
                    def sshUser = 'jenkins'
                    def hostMap = [
                        'DEV': '54.89.158.126',
                        'STAGE': '3.89.127.188',
                        'PROD': '54.172.50.79'
                    ]
                    def host = hostMap[params.ENVIRONMENT]
                    sshCommand sshUser: sshUser, host: host, command: 'cd '${checkoutDir}' && pip install -r requirements.txt'
                }
            }
        }
    }
}

def sshCommand(Map config) {
    sshCommand = "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ${config.sshUser}@${config.host} '${config.command}'"
    sh sshCommand
}
