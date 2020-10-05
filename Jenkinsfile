pipeline {
    environment {
        registry = "thedeepsyadav/devsecops-training"
        registryCredential = 'DockerHub'
        dockerImage = ''
    }
    agent any 

    stages {
        stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"  
                }
            }

        stage('Deploy to Application Server') {
            steps {
                sshagent(['AppSec']) {
                    sh "ssh root@159.65.157.103"
                    sh "docker images"
                    }
                }
            }
        }
    }