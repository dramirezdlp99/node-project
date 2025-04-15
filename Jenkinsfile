pipeline {
    agent any

    environment {
        CI = "false" // Desactiva que React trate los warnings como errores
        VERCEL_TOKEN = credentials('vercel-token') // Token de Vercel (si se usa despliegue)
    }

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Tool Install') {
            steps {
                tool name: 'Node 20', type: 'nodejs' // Asegúrate de tener Node 20 instalado
            }
        }

        stage('Clean workspace') {
            steps {
                deleteDir() // Limpia el espacio de trabajo antes de proceder
            }
        }

        stage('Checkout') {
            steps {
                git url: 'https://github.com/dramirezdlp99/node-project.git', branch: 'main' // Usa tu URL de GitHub
            }
        }

        stage('Install dependencies') {
            steps {
                bat 'npm install --legacy-peer-deps' // Instala dependencias, manejando peer-deps
            }
        }

        stage('Run tests') {
            steps {
                bat 'npm test -- --watchAll=false' // Ejecuta las pruebas sin watch
            }
        }

        stage('Build app') {
            steps {
                bat 'npm run build' // Construcción del proyecto
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline ejecutado correctamente. Build exitoso."
        }

        failure {
            echo "❌ Error en alguna etapa del pipeline. Revisar los logs."
        }

        always {
            echo "📦 Pipeline finalizado (éxito o fallo). Puedes revisar el historial."
        }
    }
}
