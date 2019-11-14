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
      }
    }
  
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          REG = "$registry:$BUILD_NUMBER"
          echo "$REG"

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
  
    stage('Stagging') {
        steps {
          script {
            kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
          }
        }
      }
    
    stage('Deployment Confirmation'){
      input {
                    message "Should we continue?"
                    ok "Yes, we should."
                    submitter "admin"
                }
            }
      steps{
        sh ''' ./patch.sh $VERSION   '''
      }
    }
    stage('Staging Deployment'){
        steps{
            script {
              kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
            }
          }
    }
  }
}
