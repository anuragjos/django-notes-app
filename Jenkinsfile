pipeline{
    agent any
    
    stages{
        stage( "Code"){
            steps{
                echo "Cloing the code"
                git url: "https://github.com/anuragjos/django-notes-app.git",
                branch: "main"
                
            }
            
        }
        stage("Build"){
            steps{
                echo "Building the code"
                sh "docker build -t note-app-todo ."
                
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing the code in to Docker Hub"
                withCredentials([usernamePassword(credentialsId: 'dockerhub_login', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                sh "docker tag note-app-todo ${env.dockerHubUser}/note-app-todo:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/note-app-todo:latest"
               }
            }
            
        }
        stage("Deploy to AWS EC2"){
            steps{
                echo "Deploying the Container"
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
        
    }
