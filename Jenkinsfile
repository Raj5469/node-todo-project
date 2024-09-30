pipeline {
    agent any

    stages {
        stage('Cleanup') {
            steps {
                cleanWs()  // This is the command to clean the workspace
            }
        }
        stage('Code') {
            steps {
                git url: "https://github.com/Raj5469/node-todo-project.git", branch: "master"
                echo 'Code cloned successfully.'
            }
        }
        stage('Build & Test') {
            steps {
                sh "docker build -t node-todo ."
                echo 'Build and test completed successfully.'
            }
        }
        stage('Push the Image') {
            steps {
                script {
                    // Correcting the typo in `withCredentials`
                    withCredentials([usernamePassword(credentialsId: "DockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker tag node-todo:latest ${env.dockerHubUser}/node-todo:latest"
                        sh "docker push ${env.dockerHubUser}/node-todo:latest"
                        echo 'Image pushed to Docker Hub successfully.'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo 'Application deployed successfully.'
            }
        }
    }
}

