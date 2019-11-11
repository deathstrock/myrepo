pipeline {
  def userInput
try {
    userInput = input(
        id: 'Proceed1', message: 'Was this successful?', parameters: [
        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']
        ])
} catch(err) { // input false
    def user = err.getCauses()[0].getUser()
    userInput = false
    echo "Aborted by: [${user}]"
}
  agent any
   if (userInput == true) {
    
     stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/deathstrock/myrepo.git'
      def doesJavaRock = input(message: 'Do you like Java?', ok: 'Yes', 
                        parameters: [booleanParam(defaultValue: true, 
                        description: 'If you like Java, just push the button',name: 'Yes?')])

      echo "Java rocks?:" + doesJavaRock
      }
      
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build registry
          //dockerImage = docker.build registry + ":$BUILD_NUMBER"
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
          //withKubeConfig([credentialsId: 'mykubeconfig', serverUrl: 'https://kubernetes.default']) 
          //sh 'kubectl apply -f myweb.yaml --kubeconfig=mykubeconfig --insecure-tls-skip-true=true'
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
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
     
     echo "this was successful"
    } 
  
