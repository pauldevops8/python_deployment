pipeline {
    agent any
    
    stages {
        stage('Build'){
            steps {
                script {
                    docker build -t myapp .
                }
            }
        }
        stage ('DockerTag'){
            steps {
                script{
                    docker tag myapp paulokie/myapp:latest
                }
            }
        }
        stage ('DockerPush'){
            steps {
                script{
                    docker push paulokie/myapp:latest
                }
            }
        }
    }
}