pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    LAST_SUCCESSFUL_BUILD = currentBuild.getPreviousBuild()
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
        print LAST_SUCCESSFUL_BUILD
        sh 'docker push priya20xenonstack/jenkins-nginx'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
