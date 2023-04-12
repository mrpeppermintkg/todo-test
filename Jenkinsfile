def img
pipeline{
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['DEV', 'STAGE', 'PROD'], description: 'Choose the environment to deploy the app to')
    }
    environment {
        registry = "kartikeyagarg/python-jenkins"
        registryCredential = 'docker-hub'
        dockerImage = ''
    }
    agent any
    
    stages{
        
        stage("build checkout") {
            steps {
                git branch:'main', url: 'https://github.com/mrpeppermintkg/SimpleFlaskUI'
            }
        }
        
        stage("Stopping Existing Container") {
            steps {
                sh returnStatus: true, script: 'docker stop $(docker ps -a | grep ${JOB_NAME} | awk \'{print $1}\')'
                sh returnStatus: true, script: 'docker rm  ${JOB_NAME}'
                sh returnStatus: true, script: 'docker stop $(docker images | grep ${registry} | awk \'{print $3}\') --force'
            }
        }  
        
        stage("build image") {
            steps {
                script {
                    img = registry + ":${env.BUILD_ID}"
                    println ("${img}")
                    dockerImage = docker.build("${img}")
                    
                }
            }
        }
        
        
        stage("Testing- running in Jenkins Node") {
            steps {
                script {
                    sh "docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
                    
                }
            }
        }
        
        stage("Push to DockerHub") {
            steps {
                script {
                    docker.withRegistry( 'https://registry.hub.docker.com ', registryCredential ) {
                        dockerImage.push()
                    }
                    
                }
            }
        }
        stage("Running in Dev") {
            when {
               expression { params.ENVIRONMENT == 'DEV' }
           }
            steps {
                script {
                       def stopcontainer = "docker stop ${JOB_NAME}"
                       def delcontName = "docker rm ${JOB_NAME}"
                       def delimages = "docker image prune -a --force" 
                       def drun = "docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
                       sshagent(['dev-server']) {
                           sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@54.89.158.126 ${stopcontainer}"
                           sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@54.89.158.126 ${delcontName}"
                           sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@54.89.158.126 ${delimages}"
                           
                           sh "ssh -o StrictHostKeyChecking=no ubuntu@54.89.158.126 ${drun}"
                    }
                }
            }
        } 
        stage("Running in Stage") {
            steps {
                script {
                       def stopcontainer = "docker stop ${JOB_NAME}"
                       def delcontName = "docker rm ${JOB_NAME}"
                       def delimages = "docker image prune -a --force" 
                       def drun = "docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
                       sshagent(['dev-server']) {
                           sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@3.89.127.188 ${stopcontainer}"
                           sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@3.89.127.188 ${delcontName}"
                           sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@3.89.127.188 ${delimages}"
                           
                           sh "ssh -o StrictHostKeyChecking=no ubuntu@3.89.127.188 ${drun}"
                    }
                }
            }
        } 
        // stage("Running in Prod") {
        //     steps {
        //         script {
        //               def stopcontainer = "docker stop ${JOB_NAME}"
        //               def delcontName = "docker rm ${JOB_NAME}"
        //               def delimages = "docker image prune -a --force" 
        //               def drun = "docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
        //               sshagent(['dev-server']) {
        //                   sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@54.172.50.79 ${stopcontainer}"
        //                   sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@54.172.50.79 ${delcontName}"
        //                   sh returnStatus: true, script: "ssh -o StrictHostKeyChecking=no ubuntu@54.172.50.79 ${delimages}"
                           
        //                   sh "ssh -o StrictHostKeyChecking=no ubuntu@54.172.50.79 ${drun}"
        //             }
        //         }
        //     }
        // } 
    }
}