pipeline{
    
    agent { label 'prod-server' }
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/Imran-Shaikh-2001/node-todo-cicd.git", branch: "master"
                
            }
        }
        stage("Code Build&Test"){
            steps{
                sh "docker build -t node-app ."
                
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"mydockercred",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                    }
            }
        }
        stage("Code Deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
