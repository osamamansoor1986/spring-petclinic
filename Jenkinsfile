pipeline {
    agent any

    environment {
        APP_PORT = '9090'
        PID_FILE = 'pid.txt'
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
                    if [ -f "${PID_FILE}" ]; then
                        OLD_PID=$(cat ${PID_FILE})
                        if ps -p $OLD_PID > /dev/null; then
                            kill $OLD_PID
                            echo "Killed existing process with PID $OLD_PID"
                        else
                            echo "No process with PID $OLD_PID running"
                        fi
                        rm -f ${PID_FILE}
                    fi

                    nohup java -jar target/*-exec.jar --server.port=${APP_PORT} > app.log 2>&1 &
                    echo $! > ${PID_FILE}
                    echo "Started Spring Petclinic with PID $(cat ${PID_FILE})"
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
