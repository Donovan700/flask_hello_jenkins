pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: python
    image: python:3.7.2
    command:
    - cat
    tty: true
"""
        }
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