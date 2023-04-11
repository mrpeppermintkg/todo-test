pipeline {
    agent any
    environment {
        PROJECT_NAME = "todo-list"
    }
    stages{
        stage('Init') {
            steps{
                script{
                    cleanWs()
                    sh """
                    git clone https://github.com/mrpeppermintkg/todo-test.git
                    cd todo-test/
                    """

                    if(env == "dev"){
                        withCredentials([string(credentialsId: 'DEV_HOST', variable: 'DEV_HOST')]){
                            HOST_IP = ${DEV_HOST}
                        }
                    }
                    else if(env == "stage"){
                        withCredentials([string(credentialsId: 'STAGE_HOST', variable: 'STAGE_HOST')]){
                            HOST_IP = ${STAGE_HOST}
                            }
                    }
                    else if(env == "prod"){
                        withCredentials([string(credentialsId: 'PROD_HOST', variable: 'PROD_HOST')]){
                            HOST_IP = ${PROD_HOST}
                            }
                    }
                    else{
                        echo "Enter valid Env"
                    }
                }
                    
            }
        }

        stage('Build'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'SSH-Key', variable: 'ssh-key')]){
                        sh """
                        echo ${ssh-key} > docker.pem
                        ls
                        """
                    }
                }
            }
        }
    }
}
