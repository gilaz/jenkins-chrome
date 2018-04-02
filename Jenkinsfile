pipeline {
  agent {
    dockerfile {
        filename '/agents/Dockerfile-chrome'
    }
  }

  stages {
    stage('versions') {
      steps {
          sh 'node --version'
          sh 'docker --version'
          sh 'docker-compose --version'
          sh 'google-chrome --version'
          sh 'ls -al'
          sh 'pwd'
          sh 'ls -al /'
      }
    }

    stage('checkout') {
      steps {
          git credentialsId: 'guidex-bitbacket', url: 'git@bitbucket.org:guideup/frontend.git'
          sh 'ls -al'
      }
    }
  }
}
