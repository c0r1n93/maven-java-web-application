pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'corinelaure/Java_Web_app:latest'
        EC2_HOST = '184.72.150.29' // Public IP of your EC2 instance
        // Assuming you have added your SSH private key in Jenkins credentials as 'jenkins (The jenkins ssh key)'
        SSH_KEY = credentials('The jenkins ssh key')
    }

    stages {
        stage('Checkout') {
            /* groovylint-disable-next-line ClosureStatementOnOpeningLineOfMultipleLineClosure */
            /* groovylint-disable-next-line ClosureStatementOnOpeningLineOfMultipleLineClosure */
            steps {
                git 'https://github.com/c0r1n93/maven-java-web-application.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker login -u corinelaure -p Corinemichaela1'
                sh "docker push ${DOCKER_IMAGE}"
            }
        }

        stage('Deploy to EC2') {
            steps {
                // Deploy using SSH command
                sh """
                ssh -i ${SSH_KEY} ec2-user@${EC2_HOST} '
                  docker pull ${DOCKER_IMAGE} &&
                  docker stop your_app_container || true &&
                  docker rm your_app_container || true &&
                  docker run -d --name your_app_container -p 8080:8080 ${DOCKER_IMAGE}
                '
                """
            }
        }
    }
}
