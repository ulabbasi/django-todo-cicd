pipeline{
    agent any
    stages{
        stage("Code"){
            steps{
            echo "Cloning Code from Repository"
            git url:"https://github.com/ulabbasi/django-todo-cicd.git", branch:"develop"
            }
            
        }
        stage("Build"){
            steps{
                echo "Building the Image"
                sh "docker build -t todo ."   
            }
            
        }
        stage("Push Image to Hub"){
            steps{
                echo "Pushing Image to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerhubpassword",usernameVariable:"dockerhubusername")]){
                    sh "docker tag todo ${env.dockerhubusername}/todo-app:latest"
                    sh "docker login -u ${env.dockerhubusername} -p ${env.dockerhubpassword}"
                    sh "docker push ${env.dockerhubusername}/todo-app:latest"
                }
            }
    
        }
        stage("Depoly"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
}
