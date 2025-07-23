pipeline {
  agent {
    docker {
      image 'docker:20.10.16-dind'
      args '--privileged'
    }
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build images') {
      steps {
        echo '📦 Construction des images Docker...'
        sh 'ls -al voting-app'  
        sh 'docker compose -f voting-app/docker-compose.yml build'
      }
    }

    stage('Run containers') {
      steps {
        echo '🚀 Démarrage de l\'application...'
        sh 'docker compose -f voting-app/docker-compose.yml up -d'
      }
    }

    stage('Check container health') {
      steps {
        echo '🩺 Vérification de l\'état des conteneurs...'
        script {
          def unhealthy = sh(script: 'docker ps --filter "health=unhealthy" --format "{{.Names}}"', returnStdout: true).trim()
          if (unhealthy) {
            error "❌ Certains conteneurs sont en erreur : ${unhealthy}"
          }
        }
      }
    }
  }

  post {
    success {
      echo '✅ Déploiement réussi.'
    }
    failure {
      echo '❌ Erreur dans le pipeline.'
    }
  }
}
