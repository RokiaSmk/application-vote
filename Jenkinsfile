pipeline {
  agent any

  stages {
    stage('Clone') {
      steps {
        echo 'Code cloné automatiquement par Jenkins depuis Git'
      }
    }

    stage('Build images') {
      steps {
        echo 'Construction des images Docker...'
        sh 'docker compose build'
      }
    }

    stage('Run containers') {
      steps {
        echo 'Démarrage de l\'application...'
        sh 'docker compose up -d'
      }
    }

    stage('Check container health') {
      steps {
        echo 'Vérification des conteneurs...'
        script {
          def result = sh(script: 'docker ps --filter "health=unhealthy" --format "{{.Names}}"', returnStdout: true).trim()
          if (result) {
            error "Certains conteneurs sont en erreur: ${result}"
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
