pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: jnlp
    image: jenkins/inbound-agent:latest
    args:
      - \$(JENKINS_SECRET)
      - \$(JENKINS_NAME)

  - name: python
    image: python:3.11-slim
    command:
      - cat
    tty: true
"""
    }
  }

  stages {
    stage('Install dependencies') {
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
