pipeline {
    agent any

    environment {
       // JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        //PATH = "${JAVA_HOME}/bin:${env.PATH}"
        IMAGE_NAME = 'petclinic'
        APP_PORT = '8081'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

     stage('Build & Test') {
    steps {
        sh '''
          java -version
          mvn clean package -DskipTests
        '''
    }
}


   

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                  docker rm -f petclinic || true
                  docker run -d \
                    --name petclinic \
                    -p ${APP_PORT}:8080 \
                    petclinic
                '''
            }
        }
    }

    post {
        success {
            echo "Petclinic deployed successfully on port ${APP_PORT}"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
