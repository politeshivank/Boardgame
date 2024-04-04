pipeline {
    agent any
    tools{
        maven 'maven'
        jdk 'java'
        
    }
    environment {
        DOCKER_CREDENTIALS = credentials('my-docker')
    }
    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', credentialsId: '36fc6a07-b46b-4e6f-b94b-69ab3be66842', url: 'https://github.com/politeshivank/Boardgame.git'
            }
        }
        stage('package build') {
            steps {
                bat "mvn -B package --file pom.xml"
            }
        }
        stage('image build') {
            steps {
                bat " docker build -t sshivnk/boardgame:latest ."
            }
        }
        stage('docker login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'my-docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]){
                        bat "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    }
                }
                
            }
        }
        stage('image push') {
            steps {
                bat "docker push sshivnk/boardgame:latest"
            }
        }
    }
}
