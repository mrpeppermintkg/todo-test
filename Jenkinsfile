pipeline{
    environment{
        DEV_HOST = credentials('DEV_HOST')
        SECRET_KEY = credentials('SSH-Key')
    }
    agent any
    stages{
            stage('Deploy'){
                steps{
                    script{
                                sh """
                                echo ${pem_key} > docker.pem
                                chmod 400 docker.pem
                                ssh -i "docker.pem" ubuntu@${dev_host}
                                """
                    }
                }
            }

        }
    }
