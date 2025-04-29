pipeline {
    agent any

    environment {
        APP_PORT = '9090'
    }

  stage('Checkout') {
    steps {
        git branch: 'main', url: 'https://github.com/osamamansoor1986/spring-petclinic.git'
    }


        stage('Build JAR') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Run App') {
            steps {
                sh 'pkill -f "spring-petclinic" || true'
                sh 'nohup java -jar target/*.jar --server.port=${APP_PORT} > app.log 2>&1 &'
            }
        }
    }

    post {
        success {
            echo "Spring Petclinic deployed at http://<your-server-ip>:${APP_PORT}"
        }
        failure {
            echo "Deployment failed. Check logs."
        }
    }
}
