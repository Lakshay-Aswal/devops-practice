pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          image = docker.build("django-app:${env.BUILD_NUMBER}")
        }
      }
    }
    stage('Run Django App in Container') {
      agent {
        docker {
          image "${image.id}"
          args '-p 8000:8000'
        }
      }
      steps {
        sh 'python manage.py migrate'
        sh 'python manage.py runserver 0.0.0.0:8000 &'
      }
    }
  }
}
