pipeline{
    environment{
        DEV_HOST = "${env.DEV_HOST}"
    }
    agent any
    stages{
        stage('Init'){
            steps{
                script{
                    cleanWs()
                    checkout scm
                    if(env.equals("dev")){
                        withCredentials([string(credentialsId: 'DEV_HOST', variable: 'dev_host')]){
                            deploy_ip = dev_host
                        }
                    }
                    else if(env.equals("stage")){
                        withCredentials([string(credentialsId: 'STAGE_HOST', variable: 'stage_host')]){
                            deploy_ip = stage_host
                        }
                    }
                    else if(env.prod){
                        withCredentials([string(credentialsId: 'PROD_HOST', variable: 'prod_host')]){
                            deploy_ip = prod_host
                        }
                    }
                    else{
                        echo "Select appropriate env"
                    }
                    }
                }
            }
            stage('Deploy'){
                steps{
                    script{
                        withCredentials([file(credentialsId: 'SSH-Key', variable: 'pem-key')]){
                            sh """
                            echo ${pem_key} > docker.pem
                            chmod 400 docker.pem
                            ssh -i "docker.pem" ubuntu@${deploy_ip}
                            """
                        }
                    }
                }
            }

        }
    }
