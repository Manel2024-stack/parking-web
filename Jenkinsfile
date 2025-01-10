pipeline {
    agent {
        node {
            label 'parking-web'
        }
    }

    stages {
        stage('Continuous integration') {
            steps {
                git branch: 'main', url: 'https://github.com/Manel2024-stack/parking-web.git'
            }
        }

        stage('Install Docker Compose') {
            steps {
                sh '''
                # Télécharger et installer Docker Compose
                curl -L "https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                chmod +x /usr/local/bin/docker-compose

                # Vérifier l'installation
                docker-compose --version
                '''
            }
        }

        stage('Build and Run with Docker Compose') {
            steps {
                sh '''
                docker-compose down || true
                docker-compose up -d --build
                '''
            }
        }

        stage('Contrôle qualité') {
            steps {
                sh '''
                sonar-scanner \
                      -Dsonar.projectKey=parking-web \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=http://192.168.232.132:9000 \
                      -Dsonar.token=sqp_c217f1e9e68b0de7c80802ffc08f773c8c0b6413
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminée.'
        }
        success {
            echo 'Déploiement réussi.'
        }
        failure {
            echo 'Échec de la pipeline.'
        }
    }
}
