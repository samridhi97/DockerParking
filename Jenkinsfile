pipeline {
    environment { 
        registry = "samridhi9719/parking_ui"
        registryCredential = 'dockerhub'
        dockerImage = '' 
    }
    agent none
    stages {
        stage('SCM Checkout') {
            agent any
            steps {
                git 'https://github.com/samridhi9719/DockerParking.git'

            }
        }
        stage('Build') {
            agent{
                docker{ image 'node:12-alpine' } 
            }
            steps {
                 sh 'npm install'
                  sh 'npm run build'

            }
        }
        stage('Build Image') {
            agent any
            steps {
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }

            }
        }
        stage('Deploy Image') {
            agent any
            steps {
               script { 
                   docker.withRegistry( '', registryCredential ) { 
                       dockerImage.push() 
                   }
               }

            }
        }
        stage('Remove Unused Docker Image') { 
            agent any
            steps{ 
                 sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        }
    }
}
