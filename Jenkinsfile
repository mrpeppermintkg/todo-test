pipeline {
  agent any
  environment {
    APP_NAME = 'my-python-app'
    // DEV_SERVER = 'dev.example.com'
    // STAGE_SERVER = 'stage.example.com'
    // PROD_SERVER = 'prod.example.com'
  }
  stages {
    stage('Build') {
      steps {
        sh 'pip3 install -r requirements.txt'
      }
    }
    stage('Test') {
      steps {
        sh 'python manage.py test'
      }
    }
    stage('Deploy to Dev') {
      when {
        branch 'dev'
      }
      steps {
        sh "ssh jenkins@54.89.158.126 'cd /var/www/${APP_NAME} && git pull && pip install -r requirements.txt && python manage.py migrate && sudo service ${APP_NAME} restart'"
    }
    }
    stage('Deploy to Stage') {
      when {
        branch 'stage'
      }
        steps {
          sh "ssh jenkins@3.89.127.188 'cd /var/www/${APP_NAME} && git pull && pip install -r requirements.txt && python manage.py migrate && sudo service ${APP_NAME} restart'"
    }

    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
         sh "ssh jenkins@54.172.50.79 'cd /var/www/${APP_NAME} && git pull && pip install -r requirements.txt && python manage.py migrate && sudo service ${APP_NAME} restart'"

      }
    }
  }
  post {
    always {
      sh 'rm -rf venv'
    }
  }
}
