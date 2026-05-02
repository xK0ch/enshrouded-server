pipeline {
  agent any

  options {
    disableConcurrentBuilds()
    timestamps()
    buildDiscarder(logRotator(numToKeepStr: '20'))
  }

  triggers {
    cron('H 4 * * *')
  }

  environment {
    HOST_USER = credentials('ENSHROUDED_HOST_USER')
    SERVER_PASSWORD = credentials('ENSHROUDED_SERVER_PASSWORD')
  }

  stages {
    stage('Deploy') {
      steps {
        sh 'docker compose -f docker-compose-enshrouded-server.yml pull'
        sh 'docker compose -f docker-compose-enshrouded-server.yml up -d --force-recreate'
      }
    }
  }

  post {
    always {
      sh 'docker image prune -f'
    }
  }
}
