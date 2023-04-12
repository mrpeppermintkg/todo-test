pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['DEV', 'STAGE', 'PROD'], description: 'Choose the environment to deploy the app to')
    }
    stages {
        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'python manage.py migrate'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def sshUser = 'jenkins'
                    def hostMap = [
                        'DEV': '54.89.158.126',
                        'STAGE': '3.89.127.188',
                        'PROD': '54.172.50.79'
                    ]
                    def host = hostMap[params.ENVIRONMENT]
                    sshCommand sshUser: sshUser, host: host, command: 'pip install -r requirements.txt'
                    sshCommand sshUser: sshUser, host: host, command: 'python manage.py migrate'
                }
            }
        }
    }
}

def sshCommand(Map config) {
    sshCommand = "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ${config.sshUser}@${config.host} '${config.command}'"
    sh sshCommand
}
