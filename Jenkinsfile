// pipeline {
//     agent any
//     stages {
//         stage('Clone') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/nhutlin/Instagram-frontend.git'
//             }
//         }
//     }
// }
pipeline {

  environment {
    dockerimagename = "nhutlinh231/frontend-k8s"
    dockerImage = "nhutlinh231/frontend-k8s"
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/nhutlin/Instagram-frontend.git'
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
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          // docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
          //   dockerImage.push("latest")
          // }
          withDockerRegistry(credentialsId: 'dockerhublogin', url: 'https://registry.hub.docker.com') {
            sh 'docker build -t nhutlinh231/frontend-k8s .'
            sh 'docker push nhutlinh231/frontend-k8s'
          }
        }
      }
    }

    // stage('Deploying App to Kubernetes') {
    //   steps {
    //     script {
    //       kubernetesDeploy(configs: "deployment.yml", kubeconfigId: "kubernetes")
    //     }
    //   }
    // }
  }

}