pipeline {
    agent any
    stages {
        stage('Supprimer le workspace') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout SCM') {
            steps {
               git branch: 'main', credentialsId: 'ksdeve-github-id', url: 'https://github.com/ksdeve/projet-Devops.git'
            }
        }
         stage('Build image docker') {
            steps {
                script{
                    sh 'docker build -t myapp-image .'
                    sh 'docker tag myapp-image kevins:myapp-image'
                }
            }
        }

        // Etape 3 - Nettoyer les containers et images existants + déployer
        stage('Nettoyage Docker et Déploiement') {
            when {
                expression { return params.DEPLOY == 'true' }
            }
            steps {
                script {
                    // Nettoyage des anciens containers/images
                    sh 'docker rm -f myapp || true'
                    sh 'docker rmi -f myapp-image || true'
                    
                    // Déployer le conteneur
                    sh 'docker run -d --name myapp -p 8088:80 myapp-image'
                    sh 'docker inspect -f "{{ .NetworkSettings.IPAddress }}" myapp'
                }
            }
        }
    }
}
