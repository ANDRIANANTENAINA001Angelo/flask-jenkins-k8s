pipeline {
    agent {
        kubernetes {
            defaultContainer 'python'
            yaml '''
                apiVersion: v1
                kind: Pod
                metadata:
                  labels:
                    some-label: flask-ci
                spec:
                  containers:
                  - name: python
                    image: python:3.11-slim
                    command:
                    - cat
                    tty: true
                '''
        }
    }

    triggers {
        pollSCM('* * * * *')  
    }

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
    }
}