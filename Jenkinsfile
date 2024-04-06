pipeline {
  environment {
    dockerimagename = "guoyecn/react-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
       // git  branch: 'main',
       //        credentialsId: 'github-credentials',
      //          url: 'https://github.com/guoyecn/jenkins-kubernetes-deployment.git'
                     echo "step 1"
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying React.js container to Kubernetes') {
          withKubeConfig([credentialsId: 'dcf2c18c-a6b0-474c-b6be-683002feeda5', serverUrl: 'https://192.168.234.30:6443']) {
               sh 'kubectl apply -f deployment.yaml'
               sh 'kubectl apply -f service.yaml'
    }
    }
  }
}
