pipeline {
    agent {
        kubernetes {
            cloud 'k8s'
            label 'flask-python-agent'
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: python
    image: python:3.10
    command:
    - cat
    tty: true
"""
        }
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Test Python') {
            steps {
                container('python') {
                    sh 'pip install -r requirements.txt || true'
                    sh 'python -m unittest discover || true'
                }
            }
        }
    }
}