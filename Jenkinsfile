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

          sh 'kubectl --token $api_token --server https://10.1.3.80:16443 --insecure-skip-tls-verify=true delete -f deployment.yaml'
          // Aplicar los cambios en el deployment
          sh 'kubectl --token $api_token --server https://10.1.3.80:16443 --insecure-skip-tls-verify=true apply -f deployment.yaml'
        }
      }
    }
  }

  post {
    success {
      echo 'Despliegue completado exitosamente!'
      // build job: 'pruebaSelenium', wait: false
    }
    failure {
      echo 'Error en el despliegue de la aplicación'
    }
  }
}
