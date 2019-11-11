pipeline {
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/deathstrock/myrepo.git'
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }

      }
    }

    stage('Push Image') {
      steps {
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }

      }
    }

    stage('Deploy App') {
      steps {
        script {
        
          withCredentials(credentialsId: 'mykubeconfig')
                        {
                            sh 'kubectl apply -f myweb.yaml --kubeconfig=$mykubeconfig --insecure-tls-skip-true=true'
                        }
          //withKubeConfig([credentialsId: 'mykubeconfig', serverUrl: 'https://kubernetes.default']) 
          //sh 'kubectl apply -f myweb.yaml --kubeconfig=mykubeconfig --insecure-tls-skip-true=true'
          //kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig --insecure-tls-skip-true=true")
        }

      }
    }

  }
  environment {
    registry = 'deathstrock47/newrepo'
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
}
