pipeline {

  environment {
    registry = "deathstrock47/newrepo"
    registryCredential = 'dockerhub'
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/deathstrock/myrepo.git'
      }
    }

    stage('Build image') {
      steps{
        sh """sudo chown root:jenkins /var/run/docker.sock"""
        sh """sudo docker build -t deathstrock47/newrepo:latest ."""
       // script {
       //   dockerImage = docker.build registry + ":latest"
       // }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            sh """sudo docker push ${registry}:latest"""
            //dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
