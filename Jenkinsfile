pipeline {
  agent any
  options { timestamps() }

  stages {

    stage('Checkout') {
      steps {
        echo '=== Checkout : récupération du code depuis Git ==='
        checkout scm
        bat 'git status'
      }
    }

    stage('Build') {
      steps {
        echo '=== Build : existence de index.html et style.css ==='
        script {
          if (!fileExists('index.html')) {
            error('Build échoué : index.html est introuvable')
          }
          echo 'index.html existe'
          if (!fileExists('style.css')) {
            error('Build échoué : style.css est introuvable')
          }
          echo 'style.css existe'
        }
        bat 'dir'
      }
    }

    stage('Test') {
      steps {
        echo '=== Test : existence des balises html et h1 ==='
        script {
          def contenu = readFile(file: 'index.html', encoding: 'UTF-8').toLowerCase()
          if (!contenu.contains('<html')) {
            error('Test échoué : la balise <html> est absente de index.html')
          }
          echo 'La balise <html> existe'
          if (!contenu.contains('<h1')) {
            error('Test échoué : la balise <h1> est absente de index.html')
          }
          echo 'La balise <h1> existe'
        }
      }
    }

    stage('Deploy') {
      steps {
        echo '=== Deploy : déploiement local ==='
        bat 'if not exist deploy mkdir deploy'
        bat 'copy /Y index.html deploy\\'
        bat 'copy /Y style.css deploy\\'
        bat 'dir deploy'
      }
    }

    stage('Archive') {
      steps {
        echo '=== Archivage des artefacts ==='
        archiveArtifacts artifacts: 'deploy/**', fingerprint: true
      }
    }
  }

  post {
    success { echo 'Build OK' }
    failure { echo 'Build KO' }
  }
}