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
         stage('Deploiement application') {
            steps {
                script{
                    sh 'docker stop myapp'                  
                    sh 'docker rm myapp'                  
                    sh 'docker run -d --name myapp --hostname myapp -p 8088:80 myapp-image'
                    sh 'docker exec myapp "ifconfig"'
                    sh 'docker inspect -f "{{ .NetworkSettings.IPAddress }}" myapp'
                }
            }
        }
    }
}
