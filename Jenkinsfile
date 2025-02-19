pipeline {
    agent any
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/roni3132/Multitier-flask-app-with-docker.git", branch: "main"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker compose up"
                // dockercompose v2 version 
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhubcredentials",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag flaskapp ${env.dockerHubUser}/flaskapp:latest"
                    sh "docker push ${env.dockerHubUser}/flaskapp:latest" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
