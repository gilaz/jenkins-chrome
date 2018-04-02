node {
  def agentImage = docker.build("agent-chrome:${env.BUILD_ID}", "-f /agents/Dockerfile-chrome ./")

  agentImage.inside('-p 6081:80 -v yarn-cache:/yarn-cache') { c ->

    stage('clean') {
      // cleanWs()
    }

    stage('versions') {
      sh 'node --version'
      sh 'docker --version'
      sh 'docker-compose --version'
      sh 'google-chrome --version'
      sh 'yarn --version'
    }

    stage('checkout') {
      git credentialsId: 'guidex-bitbacket', url: 'git@bitbucket.org:guideup/frontend.git'
    }

    stage('dependencies') {
      sh 'YARN_CACHE_FOLDER=/yarn-cache yarn install --frozen-lockfile'
    }

    stage('login to docker hub') {
      withCredentials([usernamePassword(credentialsId: 'hub.lite.network', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
        sh 'echo "$DOCKER_PASSWORD" | docker login hub.lite.network -u "$DOCKER_USERNAME" --password-stdin'
      }
    }

    stage('pull docker images') {
      sh 'docker-compose pull'
    }

    stage('up') {
      sh 'docker-compose up -d db'
      sleep time: 2, unit: 'MINUTES'
      sh 'docker-compose up -d'
    }

    stage('e2e') {
      try {
        sh 'CI=true yarn e2e'
      }
      catch (error) {
        sh 'docker-compose down'
        throw error
      }
    }

    post {
      always {
        sh 'docker-compose down'
        // cleanWs()
      }
    }
  }
}
