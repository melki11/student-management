pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Récupération du projet depuis GitHub...'
                git branch: 'main', url: 'https://github.com/melki11/student-management'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Exécution : mvn clean package'
                bat 'mvn clean package'
            }
        }
    }

    post {
        success {
            echo '✅ Build terminé ! Le livrable se trouve dans target/'
        }
        failure {
            echo '❌ Erreur dans le build.'
        }
    }
}
