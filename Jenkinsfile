// pipeline{
//     agent {label "dev"}
//     stages{
//         stage("Clone Code"){
//             steps{
//                 git url: "https://github.com/riyazmohd02/React-django.git", branch: "main"
//             }
//         }
//         stage("Build and Test") {
//             steps{
//                 sh "docker build . -t django-todo-app"
//             }
//         }
//         stage("Login and Push image")
//         {
//             steps{
//                 withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPassword",usernameVariable:"dockerhubUsername")]){
//                     sh """
//                     #!/bin/sh
//                     docker tag django-todo-app ${env.dockerhubUsername}/django-todo-app:latest
//                     docker login -u ${env.dockerhubUsername} -p ${env.dockerhubPassword}
//                     docker push ${env.dockerhubUsername}/django-todo-app:latest
//                     """
//                 }
//             }
//         }
//         stage("Deploy") {
//             steps{
//                 sh "docker-compose down && docker-compose up -d"
//             }
//         }
//     }
// }
pipeline {
    agent any
    environment {
        DOCKER_USERNAME = '9966167557'
        DOCKER_PASSWORD = 'Chanti77#'
        DOCKER_REGISTRY = 'docker.io'  // Docker Hub URL
        DOCKER_IMAGE_NAME = 'my-note-app'
        DOCKER_IMAGE_TAG = 'latest'
        DOCKER_FULL_IMAGE_NAME = "${DOCKER_REGISTRY}/${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/riyazmohd02/React-django.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the image'
                sh "docker build -t ${DOCKER_IMAGE_NAME} ."
            }
        }

        stage('Push to Registry') {
            steps {
                script {
                    // Log in to the Docker registry
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin ${DOCKER_REGISTRY}"

                    // Remove the old Docker image if it exists
                    sh "docker rm -f ${DOCKER_IMAGE_NAME} || true"

                    // Tag the Docker image
                    sh "docker tag ${DOCKER_IMAGE_NAME} ${DOCKER_FULL_IMAGE_NAME}"

                    // Push the Docker image to the registry
                    sh "docker push ${DOCKER_FULL_IMAGE_NAME}"
                }
            }
        }
    }
}
