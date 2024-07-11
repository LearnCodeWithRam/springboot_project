pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/LearnCodeWithRam/springboot_project.git'
            }
        }

        stage('Build Jar') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("genaiihub24/my-docker:springboot-v2:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop -f springboot-v2 || true'
                sh 'docker rm -f springboot-v2 || true'
                sh 'docker run -d --restart unless-stopped --name springboot-v2 -p 3000:8081 genaiihub24/my-docker:springboot-v2:latest'
            }
        }
    }
}
