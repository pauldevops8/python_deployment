pipeline {
    agent {
        kubernetes {
            inheritFrom 'default'
            yamlMergeStrategy merge()
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                  containers:
                  - name: "jenkins"
                    image: "pauldevops/jenkins:latest"
                    command:
                    - cat
                    tty: true
                    volumeMounts:
                    - name: "docker-sock"
                      mountPath: "/var/run/docker.sock"
                    securityContext:
                      privileged: true
                  volumes:
                  - name: "docker-sock"
                    hostPath:
                      path: "/var/run/docker.sock"
            '''
        }
    }
    
    stages {
        stage('Build'){
            steps {
                container ('jenkins'){
                script {
                  sh 'ls -ll'
                  sh  'docker build -t pythonapp:latest .'
                  sh 'docker tag pythonapp:latest pauldevops/pythonapp:latest'
                }
            }
            }
        }
          stage('Push'){
            steps {
                container ('jenkins'){
                  withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                  script {
                  sh 'docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}'
                  sh 'docker push'
                }
        }
            }
            }
        }
    }
}