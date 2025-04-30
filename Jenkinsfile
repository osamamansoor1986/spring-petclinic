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
                sh '''
                    echo "Stopping any existing Spring Petclinic process..."
                    pkill -f spring-petclinic || true

                    echo "Starting Spring Petclinic on port $APP_PORT..."
                    nohup java -jar target/*-exec.jar --server.port=$APP_PORT > app.log 2>&1 &

                    sleep 5

                    echo "Checking if the application started..."
                    pgrep -f spring-petclinic > /dev/null
                '''
            }
        }
    }

    post {
        success {
            echo "Spring Petclinic deployed successfully at http://10.1.0.123:${APP_PORT}"
        }
        failure {
            echo "Deployment failed. Check logs."
        }
    }
}

