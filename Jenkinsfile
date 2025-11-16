pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo '📦 Récupération du projet depuis GitHub...'
                git branch: 'main', 
                    url: 'https://github.com/melki11/student-management'
            }
        }
        
        stage('Build') {
            steps {
                echo '🔨 Build avec profil test (tests désactivés temporairement)...'
                bat 'mvn clean package -Dspring.profiles.active=test -DskipTests'
            }
        }
        
        stage('Archive Artifact') {
            steps {
                echo '📦 Archivage du JAR...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
    
    post {
        success {
            echo '✅ SUCCÈS : Build réussi (tests temporairement désactivés)'
        }
        failure {
            echo '❌ ÉCHEC : Build échoué'
        }
    }
}