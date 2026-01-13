pipeline {
  agent {
    kubernetes {
      namespace 'jenkins'
      defaultContainer 'python'
      yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: python
    image: python:3.11-slim
    command:
    - sh
    - -c
    - cat
    tty: true

  - name: docker
    image: docker:27
    command:
    - sh
    - -c
    - cat
    tty: true
    volumeMounts:
    - name: docker-sock
      mountPath: /var/run/docker.sock

  - name: kubectl
    image: bitnami/kubectl:latest
    command:
    - sh
    - -c
    - cat
    tty: true

  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
'''
    }
  }

  stages {

    stage('Install dependencies') {
      steps {
        container('python') {
          sh '''
          pip install --upgrade pip
          pip install -r requirements.txt
          '''
        }
      }
    }

    stage('Run tests') {
      steps {
        container('python') {
          sh 'python test_app.py'
        }
      }
    }

    stage('Push Docker image') {
      steps { 
        container('docker') {
          sh '''
          docker tag localhost:4000/flask-app:latest localhost:4000/flask-app:latest
          docker push localhost:4000/flask-app:latest
          '''
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        container('kubectl') {
          sh '''
          kubectl apply -f k8s -n jenkins
          kubectl rollout status deployment/flask-app -n jenkins
          '''
        }
      }
    }
  }
}
