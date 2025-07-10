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
                scp -i ~/.ssh/id_rsa complete/target/${JAR_NAME} $DEPLOY_HOST:$DEPLOY_DIR/$JAR_NAME
                ssh -i ~/.ssh/id_rsa $DEPLOY_HOST "pkill -f 'java -jar' || true && setsid java -jar $DEPLOY_DIR/$JAR_NAME --server.address=0.0.0.0 --server.port=8080 > $DEPLOY_DIR/app.log 2>&1 < /dev/null &"
                '''
            }
}
    }
}
