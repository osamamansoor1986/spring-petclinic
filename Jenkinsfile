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
                    if pgrep -f "spring-petclinic"; then
                        pkill -f "spring-petclinic"
                        echo "Previous Spring Petclinic process killed."
                    else
                        echo "No previous process found."
                    fi

                    nohup java -jar target/*-exec.jar --server.port=${APP_PORT} > app.log 2>&1 &
                    echo $! > pid.txt
                    echo "New Spring Petclinic started with PID $(cat pid.txt)"
                '''
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
