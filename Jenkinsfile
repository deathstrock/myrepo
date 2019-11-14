pipeline {

  environment {
    registry = "deathstrock47/newrepo"
    dockerImage = ""
    registryCredential = "dockerhub"
  }

  agent any

  stages {
  //  stage ('confirmation') {
  //    steps{
  //    input('do you wish to continue')
  //    }
  //  }
    stage('Checkout Source') {
      steps {
        git 'https://github.com/deathstrock/myrepo.git'
        //sh """ git checkout $BRANCH """
      }
    }
  
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          BUILD = "$registry:$BUILD_NUMBER"
        }
      }
    }
  
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" , registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    //stage('merge to staging'){
    //  
    //}
    stage('Stagging') {
        steps {
          script{
            withKubeConfig([credentialsId: 'mykubeconfig', serverUrl: 'https://34.69.248.211']) {
            sh 'kubectl apply -f myweb.yaml --namespace jenkins'
          }
          }

          //script {
          //  kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
          //}
        }
      }

    stage('Deployment Confirmation'){
      input {
            message "Should we continue?"
            ok "Yes, we should."
            submitter "admin"
      }
      steps{
        sh ''' ./patch.sh $VERSION   '''
      }
    }
    //stage('Staging Deployment'){
    //  steps{
    //      script {
    //        //kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
    //      }
    //    }
    //  }
  }
}
