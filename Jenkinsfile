pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo "📥 Récupération du projet depuis GitHub..."
                git branch: 'main', url: 'https://github.com/melki11/student-management'
            }
        }
        
        stage('Build') {
            steps {
                echo "🔨 Build avec profil test..."
                bat 'mvn clean package -Dspring.profiles.active=test'
            }
        }
    }
    
    post {
        success {
            echo "✅ SUCCÈS : Build réussi !"
        }
        failure {
            echo "❌ ÉCHEC : Build échoué"
        }
    }
}