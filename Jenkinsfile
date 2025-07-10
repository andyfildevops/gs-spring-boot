pipeline {
    agent any

    tools {
        maven 'Maven 3'
    }

    environment {
        DEPLOY_HOST = 'ubuntu@3.90.0.130'
        DEPLOY_DIR = '/home/ubuntu/app'
        JAR_NAME = 'spring-boot-complete-0.0.1-SNAPSHOT.jar'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/andyfildevops/gs-spring-boot.git', branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                dir('complete') {
                    sh 'mvn clean install'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                echo "[INFO] Copying JAR to EC2"
                /usr/bin/scp -i $LOCAL_KEY complete/target/$JAR_NAME $DEPLOY_HOST:$DEPLOY_DIR/$JAR_NAME

                echo "[INFO] Killing previous Java process"
                /usr/bin/ssh -i $LOCAL_KEY $DEPLOY_HOST "pkill -f 'java -jar' || true"

                echo "[INFO] Starting application via nohup"
                /usr/bin/ssh -i $LOCAL_KEY $DEPLOY_HOST "nohup java -jar $DEPLOY_DIR/$JAR_NAME --server.address=0.0.0.0 --server.port=8080 > $DEPLOY_DIR/app.log 2>&1 &"
                '''
            }
}
    }
}
