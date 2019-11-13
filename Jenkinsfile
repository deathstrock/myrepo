pipeline {

  environment {
    registry = "deathstrock47/newrepo"
    dockerImage = ""
    registryCredential = "dockerhub"
  }

  agent any

  stages {
    //stage ('confirmation') {
    //  steps{
    //  input('do you wish to contineu')
    //  }
    //}
    stage('Checkout Source') {
      steps {
        git 'https://github.com/deathstrock/myrepo.git'
      }
    }

    //stage('Build image') {
    //  steps{
    //    script {
    //      dockerImage = docker.build registry + ":$BUILD_NUMBER"
    //    }
    //  }
    //}

    //stage('Push Image') {
    //  steps{
    //    script {
    //      docker.withRegistry( "" , registryCredential ) {
    //        dockerImage.push()
    //      }
    //    }
    //  }
    //}

    //stage('Deploy App') {
    //  steps {
    //    script {
    //      kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
    //    }
    //  }
    //}
    stage('Deployment Confirmation'){
      steps{
        input("Deploy changes to production?")
      }
    }
    stage('Staging Deployment'){
      steps{
          withCredentials([kubeconfigContent(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG_CONTENT')]) {
          sh '''echo "$KUBECONFIG_CONTENT"'''
        }
      }
    }
    //stage('Production Deployment'){
    //
    //
    //}
  }

}
