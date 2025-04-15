pipeline {
    agent any

    environment {
        // Variables de entorno
        VERCEL_TOKEN = credentials('vercel-token')  // Asegúrate de tener este secreto configurado en Jenkins
        NOMBRE_ENTORNO = ""  // Opcional: Define tu entorno si lo necesitas, sino se dejará vacío
    }

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Declarative: Tool Install') {
            steps {
                script {
                    // Configuración de herramientas necesarias si se requiere alguna
                    // Si tienes Node.js o alguna herramienta extra, agrégala aquí
                }
            }
        }

        stage('Clean workspace') {
            steps {
                deleteDir()  // Limpia el directorio de trabajo
            }
        }

        stage('Checkout') {
            steps {
                checkout scm  // Vuelve a hacer el checkout del código si es necesario
            }
        }

        stage('Install dependencies') {
            steps {
                script {
                    bat 'npm install --legacy-peer-deps'  // Instala las dependencias con --legacy-peer-deps
                }
            }
        }

        stage('Run tests') {
            steps {
                script {
                    bat 'npm test -- --watchAll=false'  // Ejecuta las pruebas
                }
            }
        }

        stage('Build app') {
            steps {
                script {
                    bat 'npm run build'  // Construye la aplicación para producción
                }
            }
        }

        stage('Deploy to Vercel') {
            steps {
                script {
                    // Despliega en Vercel con la opción --yes para evitar la confirmación
                    bat "npx vercel --prod --token=${VERCEL_TOKEN} --yes"
                }
            }
        }

        stage('Declarative: Post Actions') {
            steps {
                echo "❌ El pipeline falló. Revisa los logs."  // Aquí puedes personalizar el mensaje post-build si es necesario
            }
        }
    }

    post {
        always {
            // Esta sección se ejecuta al final, independientemente de si el pipeline fue exitoso o falló.
            echo "Pipeline completo"
        }

        success {
            // Si el pipeline fue exitoso, puedes agregar tareas adicionales aquí
            echo "Pipeline ejecutado con éxito"
        }

        failure {
            // Si el pipeline falló, puedes agregar tareas aquí para manejar el error
            echo "El pipeline falló, revisa los logs"
        }
    }
}
