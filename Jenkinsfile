pipeline {
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code..."
                git url: "https://github.com/mahesh-bora/NotesApp.git", branch: "main"
            }
        }
        stage("Build Image") {
            steps {
                echo "Building the image..."
                sh "docker build -t notes-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                    sh "docker tag notes-app ${dockerHubUser}/notes-app:latest"
                    sh "docker push ${dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy Container") {
            steps {
                echo "Deploying the container..."
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
