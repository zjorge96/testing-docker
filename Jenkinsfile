pipeline {
    agent any

    environment {
        F_VERSION = '1.0.0' // Semantic versioning for the frontend
        B_VERSION = '1.0.0' // Semantic versioning for the backend
    }
    
    tools { // Used to set tool configuration tools to use in the pipeline
        // go "${GO_NAME}"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh "docker build ./frontend -t zjliatrio/realworld_frontend:${F_VERSION}" // Builds the frontend image
                sh "docker build ./backend -t zjliatrio/realworld_backend:${B_VERSION}" // Builds the backend image
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh "docker run -d zjliatrio/realworld_frontend:${F_VERSION}" // Runs the frontend image
                sh "docker run -d zjliatrio/realworld_backend:${B_VERSION}" // Runs the backend image

            }
        }
        stage('Deploy') {
            environment {
                // GITHUB_TOKEN = credentials('GitLogin') // Make sure to set a PAT in credentials
            }
            steps {
                echo 'Deploying..'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}" // Logs into dockerhub
                    sh "docker push zjliatrio/realworld_frontend:${F_VERSION}" // Pushes the frontend image to DockerHub
                    sh "docker push zjliatrio/realworld_backend:${B_VERSION}" // Pushes the backend image to DockerHub
                }
            }
        }
        stage('Cleanup') {
            steps {
                echo 'Cleaning up..'
                sh "docker rm $(docker ps -a -q)" // Removes all containers
                sh "docker rmi zjliatrio/realworld_frontend" // Removes frontend image
                sh "docker rmi zjliatrio/realworld_backend" // Removes backend image
                // sh "docker rmi $(docker images -q)" // Removes all images
            }
        }
    }
}