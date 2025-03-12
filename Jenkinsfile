pipeline {
  agent any
  stages {
    stage('clone repository') {
      steps {
        sh '''java -version
mvn --version
git --version'''
      }
    }

    stage('Deploy billing App') {
      steps {
        withCredentials(bindings: [
          string(credentialsId: 'k8s-minikube', variable: 'api_token')
        ]) {
          // Aplicar los cambios en el deployment
          sh 'kubectl --token $api_token --server https://10.1.3.80:16443 --insecure-skip-tls-verify=true replace -f deployment.yaml'

          // Forzar reinicio para asegurar que la nueva imagen se use
          //sh 'kubectl --token $api_token --server https://10.1.3.80:16443 --insecure-skip-tls-verify=true rollout restart deployment crud-app'
        }
      }
    }
  }
}