pipeline {
    agent {
        kubernetes {
            defaultContainer 'python'
            yaml '''
apiVersion: v1
kind: Pod
spec:
  nodeSelector:
    kubernetes.io/hostname: minikube
  containers:
  - name: python
    image: python:3.11-slim
    command:
    - cat
    tty: true
  - name: docker
    image: docker:27-dind
    securityContext:
      privileged: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
'''
        }
    }

    // triggers {
    //     pollSCM('* * * * *')
    // }

    stages {
        stage('Install dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run tests') {
            steps {
                sh 'python test_app.py'   
            }
        }

        stage('Build & Push Docker image') {
            steps {
                container('docker') {
                    sh '''
                    docker build -t localhost:4000/flask_hello:latest .
                    docker push localhost:4000/flask_hello:latest
                    '''
                }
            }
        }
    }
}