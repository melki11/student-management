pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'ahmedmelki/student-management'
        DOCKER_TAG = "${env.BUILD_ID}"
        DOCKERHUB_CREDENTIALS = 'student-management-docker'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '📦 Récupération du projet depuis GitHub...'
                git branch: 'main', url: 'https://github.com/melki11/student-management'
            }
        }
        
        stage('Build') {
            steps {
                echo '🔨 Build Java...'
                bat 'mvn clean package -DskipTests'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo '🐳 Construction Docker Image...'
                bat 'docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .'
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                echo '📤 Push vers DockerHub...'
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'student-management-docker',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {
                        bat 'echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin'
                        bat 'docker push ${DOCKER_IMAGE}:${DOCKER_TAG}'
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ SUCCÈS : Image Docker créée et poussée sur DockerHub!'
        }
        failure {
            echo '❌ ÉCHEC : Pipeline échoué'
        }
    }
}