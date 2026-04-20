pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-jenkins-app"
        CONTAINER_NAME = "flask-jenkins-container"
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                checkout scm
            }
        }

        stage('Verificar archivos') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                sh 'ls -la app'
            }
        }

        stage('Construir imagen Docker') {
            steps {
                sh 'docker build -t $IMAGE_NAME ./app'
            }
        }

        stage('Detener contenedor anterior') {
            steps {
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
            }
        }

        stage('Ejecutar contenedor') {
            steps {
                sh 'docker run -d --name $CONTAINER_NAME -p 5000:5000 $IMAGE_NAME'
            }
        }

        stage('Validar despliegue') {
            steps {
                sh 'docker ps'
                sh 'curl http://localhost:5000/health'
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado correctamente'
        }
        failure {
            echo 'Pipeline falló'
        }
    }
}
