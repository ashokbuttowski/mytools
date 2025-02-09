pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'pwd'
        sh 'cd /home/mytools'
        sh 'docker build -t my-tools .'
        sh 'docker tag my-tools $DOCKER_MYTOOLS_IMAGEURL'
      }
    }
   /* stage('Test') {
      steps {
        sh 'docker run my-flask-app python -m pytest app/tests/'
      }
    } */
    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
          sh 'docker push $DOCKER_MYTOOLS_IMAGEURL'
        }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}