pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    IMAGE_NAME = "jenkins-nginx"
    DOCKERHUB_REPO = "priya20xenonstack"
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
          print lastSuccessfulBuild
          def newTag = lastSuccessfulBuild + 1
          currentBuild.displayName = "#${newTag}"
          $BUILD_ID = "#${newTag}"
          
          print newTag
          def dockerImage = "${DOCKERHUB_REPO}/${IMAGE_NAME}:${newTag}" 
          print dockerImage
          //sh "docker build -t ${dockerImage} ."
          sh "docker tag ${IMAGE_NAME} ${dockerImage}"
          sh "docker push ${dockerImage}"
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
