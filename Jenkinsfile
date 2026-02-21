pipeline {

    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:

  - name: docker
    image: docker:24.0
    command:
    - cat
    tty: true
    env:
    - name: DOCKER_HOST
      value: tcp://localhost:2375

  - name: dind
    image: docker:24.0-dind
    securityContext:
      privileged: true
    env:
    - name: DOCKER_TLS_CERTDIR
      value: ""
    args:
    - --host=tcp://0.0.0.0:2375
    - --host=unix:///var/run/docker.sock
"""
        }
    }

    environment {
        IMAGE_TAG = "0.${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'http://192.168.88.20:3000/anderson/simplePythonFlask.git'
            }
        }

        stage('Build') {
            steps {
                container('docker') {
                    sh 'sleep 10'   // ⬅ importante para dar tempo do daemon subir
                    sh 'docker info'
                    sh "docker build -t simple-python-flask:${IMAGE_TAG} ."
                }
            }
        }
    }
}

