podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'kubectl', image: 'gcr.io/cloud-builders/kubectl', command: 'cat', ttyEnabled: true)
  ]
  ) {
    node('mypod') {
        stage('Kubectl testing with k8s cluster') {
            container('kubectl') {
                sh("kubectl get pods --all-namespaces")
            }
        }
    }
}
