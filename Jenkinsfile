pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t priya20xenonstack/jenkins-nginx .'
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
          print "hello"
          print lastSuccessfulBuild
          sh 'docker push priya20xenonstack/jenkins-nginx'
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
