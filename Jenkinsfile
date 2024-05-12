pipeline {
    agent any
    
    stages {
        stage("Code"){
            steps {
                echo "Clonning the Code!"
                git url: "https://github.com/hanzalawebdev/django-notes-app.git", branch: "main"
            }
        }
        
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-notes-app ."
            }
        }
        
        stage("Push to DockerHub"){
            steps {
                echo "Image push to DockerHub"
                withCredentials([usernamePassword(credentialsId: "DockerHub-Login", usernameVariable: "DockerHubUser", passwordVariable: "DockerHubPass")]) {
                    sh "docker tag my-notes-app ${env.DockerHubUser}/my-notes-app:latest"
                    sh "docker login -p ${env.DockerHubPass} -u ${env.DockerHubUser}"
                    sh "docker push ${env.DockerHubUser}/my-notes-app:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps {
                echo "Deploy a Container"
                sh "docker compose down && docker compose up -d"
                
            }
        }
    }
}
