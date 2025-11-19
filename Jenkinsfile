pipeline {
    agent any
    
    environment {
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
                script {
                    bat "docker build -t ahmedmelki/student-management:${env.BUILD_ID} ."
                }
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
                        bat "echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin"
                        bat "docker push ahmedmelki/student-management:${env.BUILD_ID}"
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