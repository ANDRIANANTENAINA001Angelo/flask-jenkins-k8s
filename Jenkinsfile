pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: python
    image: python:3.11-slim
    command:
    - sh
    - -c
    - "sleep 999999"
"""
    }
  }

  stages {
    stage('Install deps') {
      steps {
        container('python') {
          sh 'pip install -r requirements.txt'
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
  }
}
