pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command:
    - sleep
    args:
    - 9999999
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
  - name: kubectl
    image: bitnami/kubectl:latest
    command:
    - sleep
    args:
    - 9999999
  volumes:
  - name: docker-config
    secret:
      secretName: dockerhub-secret
      items:
        - key: .dockerconfigjson
          path: config.json
"""
        }
    }

    environment {
        IMAGE_NAME = 'roopan28/docker-webapp'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push with Kaniko') {
            steps {
                container('kaniko') {
                    sh """
                    /kaniko/executor --context `pwd` \
                    --destination=${IMAGE_NAME}:${IMAGE_TAG} \
                    --destination=${IMAGE_NAME}:latest
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                container('kubectl') {
                    sh "kubectl set image deployment/docker-webapp docker-webapp=${IMAGE_NAME}:${IMAGE_TAG} --record"
                }
            }
        }
    }
}
