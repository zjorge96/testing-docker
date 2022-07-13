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
                sh 'make test' // Runs tests through go
            }
        }
        stage('Deploy') {
            environment {
                GITHUB_TOKEN = credentials('GitLogin') // Make sure to set a PAT in credentials
            }
            steps {
                script {
                    if (env.BRANCH_NAME == 'master') {
                        echo 'Deploying..'
                        sh 'make release' // Runs goreleaser and releases the tag

                    } else {
                        echo 'Not master branch. Nothing to deploy.'
                    }
                }
                
            }
        }
    }
}