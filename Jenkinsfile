pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    IMAGE_NAME = "jenkins-nginx"
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t ${IMAGE_NAME} .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        script{
          def lastSuccessfulBuild = currentBuild.getPreviousSuccessfulBuild()?.number ?:0
          def newTag = lastSuccessfulBuild + 1
          def dockerImage = "${IMAGE_NAME}:${newTag}" 
          sh 'docker push priya20xenonstack/${dockerImage}'
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
