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
                    sh 'docker stop monapp'                  
                    sh 'docker rm monapp'                  
                    sh 'docker run -d --name monapp --hostname monapp -p 8099:80 myapp-image'
                    sh 'docker exec monapp "ifconfig"'
                }
            }
        }
    }
}
