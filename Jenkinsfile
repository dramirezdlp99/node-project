pipeline {
  agent any

  environment {
    CI = "false" // Para que React no trate los warnings como errores
    VERCEL_TOKEN = credentials('vercel-token') // Asegúrate que esta credencial exista en Jenkins
  }

  tools {
    nodejs 'Node 20' // Usa el nombre exacto como lo configuraste en Jenkins
  }

  stages {

    stage('Clean workspace') {
      steps {
        deleteDir()
      }
    }

    stage('Checkout') {
      steps {
        git url: 'https://github.com/dramirezdlp99/node-project.git', branch: 'main'
      }
    }

    stage('Install dependencies') {
      steps {
        bat 'npm install --legacy-peer-deps'
      }
    }

    stage('Run tests') {
      steps {
        bat 'npm test -- --watchAll=false'
      }
    }

    stage('Build app') {
      steps {
        bat 'npm run build'
      }
    }

    stage('Deploy to Vercel') {
      steps {
        bat "npx vercel --prod --token=%VERCEL_TOKEN%"
      }
    }
  }

  post {
    success {
      echo '✅ El pipeline se ejecutó correctamente.'
    }
    failure {
      echo '❌ El pipeline falló. Revisa los logs.'
    }
  }
}

