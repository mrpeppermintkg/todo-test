pipeline{
    environment{
        DEV_HOST = "${env.DEV_HOST}"
    }
    agent any
    stages{
            stage('Deploy'){
                steps{
                    script{
                        withCredentials([string(credentialsId: 'DEV_HOST', variable: 'dev_host')]){
                            withCredentials([file(credentialsId: 'SSH-Key', variable: 'pem-key')]){
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

        }
    }
