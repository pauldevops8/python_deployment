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
                  - name: "docker"
                    image: "pauldevops/custom-jenkins-docker:latest"
                    ports:
                    - containerPort: 8080
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
                container ('docker'){
                script {
                  sh  'docker build -t pythonapp:latest .'
                }
            }
            }
        }
    }
}