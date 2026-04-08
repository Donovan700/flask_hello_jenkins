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

  - name: docker
    image: docker
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock

  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
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
                    sh 'pip install -r requirements.txt'
                    sh 'python -m unittest discover'
                }
            }
        }

        stage('Build image') {
            steps {
                container('docker') {
                    sh "docker version"
                    sh "docker build -t localhost:32000/pythontest:latest ."
                    sh "docker push localhost:32000/pythontest:latest"
                }
            }
        }
    }
}