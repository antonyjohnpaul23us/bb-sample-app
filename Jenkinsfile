pipeline {
    agent {
        kubernetes {
            label 'kaniko'
            yaml """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: helm
    image: alpine/helm
    command:
    - cat
    tty: true
  - name: kaniko
    image: gcr.io/kaniko-project/executor@sha256:f652f28537fa76e8f4f9393de13a064f0206003c451ce2ad6e4359fd5a21acbc
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: registry-credentials
          items:
            - key: .dockerconfigjson
              path: config.json
"""
        }
    }
    stages {
        stage('Make and Push Image - bb-Sample-app') {
            steps {
                container(name: 'kaniko', shell: '/busybox/sh') {
                    sh '/kaniko/executor --context=git://github.com/antonyjohnpaul23us/testq.git \
                      --destination=antonyjohnpaul23usgmail/bb-sample-app \
                      --dockerfile=Dockerfile\
                      -v=debug'
                }
            }
        }
        stage('Helm Deploy - bb-Sample-app') {
            steps {
                container(name: 'helm', shell: '/bin/sh') {
                    sh '''
                    helm repo add bitnami https://charts.bitnami.com/bitnami
                    helm repo update
                    helm upgrade --install my-release bitnami/tomcat --namespace=jenkins -f helm_overwrite_tomcat_values.yaml --set tomcatPassword=tomcat
                    '''
                }
            }
        }        
    }
}
