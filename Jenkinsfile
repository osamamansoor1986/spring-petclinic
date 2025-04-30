pipeline {
    agent any

    environment {
        APP_PORT = '9090'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/osamamansoor1986/spring-petclinic.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Run App') {
            steps {
                // Kill previous instance if running
                sh 'pkill -f "spring-petclinic" || true'

                // Start the new application in background
                sh 'nohup java -jar target/*-exec.jar --server.port=${APP_PORT} > app.log 2>&1 &'
            }
        }
    }

    post {
        success {
            echo "Spring Petclinic deployed successfully at https://10.1.0.123:${APP_PORT}"
        }
        failure {
            echo "Deployment failed. Check logs."
        }
    }
}
