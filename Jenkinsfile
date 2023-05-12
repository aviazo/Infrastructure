pipeline {

    agent { 
        label 'docker' 
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker_hub-aviazo')
  }

    triggers {
        pollSCM '* * * * *'
    }

    stages {
         stage('checkout code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [url: 'https://github.com/aviazo/hello-world-war.git']])
                echo 'Git Checkout Completed'
            }
        }
    
    
         stage('Build') {
            steps {
                script {
                    //def mvnHome = tool name: 'maven', type: 'maven'
                    sh '''mvn clean install'''
                    sh '''docker build . -t aviazo/hello-world-war_mvn:${BUILD_ID}'''
                    echo 'Build Image Completed'
                }
            }
        }
         stage('Login') {
            steps {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    echo 'Login Completed'
            }
    
         stage('Build and Push To Docker Image') {
            steps {
                        sh '''docker push aviazo/hello-world-war_mvn:${BUILD_ID}'''
            }
           }
        
          }       
        } 
    } 
