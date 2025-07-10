pipeline {
    agent any

    tools {
        maven 'Maven 3'
    }

    environment {
        DEPLOY_HOST = 'ubuntu@3.90.0.130'
        DEPLOY_DIR = '/home/ubuntu/app'
        JAR_NAME = 'app.jar'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/andyfildevops/gs-spring-boot.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                scp -i ~/.ssh/id_rsa target/*.jar $DEPLOY_HOST:$DEPLOY_DIR/$JAR_NAME
                ssh -i ~/.ssh/id_rsa $DEPLOY_HOST << EOF
                  pkill -f "java -jar" || true
                  nohup java -jar $DEPLOY_DIR/$JAR_NAME > $DEPLOY_DIR/app.log 2>&1 &
                EOF
                '''
            }
        }
    }
}
