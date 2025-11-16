pipeline {
    agent any
    
    tools {
        maven 'M3'  // Assurez-vous que Maven est configuré dans Jenkins
        jdk 'jdk17' // Ou la version Java que vous utilisez
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo "📥 Récupération du projet depuis GitHub..."
                git branch: 'main', url: 'https://github.com/melki11/student-management'
            }
        }
        
        stage('Build et Tests') {
            steps {
                echo "🔨 Compilation et exécution des tests avec profil test..."
                bat 'mvn clean package -Dspring.profiles.active=test'
            }
        }
        
        stage('Rapport des Tests') {
            steps {
                echo "📊 Génération du rapport des tests..."
                junit 'target/surefire-reports/*.xml'
            }
        }
    }
    
    post {
        always {
            echo "🏗️ Pipeline terminée - Archivage des résultats..."
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        success {
            echo "✅ SUCCÈS : Build et tests réussis !"
            emailext (
                subject: "✅ SUCCÈS : Build Jenkins - ${env.JOB_NAME}",
                body: "Le build ${env.BUILD_NUMBER} a réussi!\n\n${env.BUILD_URL}",
                to: "colordwitch@gmail.com"  // Remplacez par votre email
            )
        }
        failure {
            echo "❌ ÉCHEC : Erreur dans le build ou les tests"
            emailext (
                subject: "❌ ÉCHEC : Build Jenkins - ${env.JOB_NAME}",
                body: "Le build ${env.BUILD_NUMBER} a échoué.\n\n${env.BUILD_URL}",
                to: "colordwitch@gmail.com"  // Remplacez par votre email
            )
        }
        unstable {
            echo "⚠️ INSTABLE : Certains tests ont échoué"
        }
    }
}