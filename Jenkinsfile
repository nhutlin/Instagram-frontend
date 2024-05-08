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
    dockerImage = ""
  }

  agent any

  tools {
    nodejs '21.7.1'
  }

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/nhutlin/Instagram-frontend.git'
      }
    }

    stage('Build') {
      steps {
        sh 'npm version'
        sh 'cd /var/jenkins_home/workspace/FE-Instagram-CICD && npm install && npm run build'
        echo 'Run build successfully...'
      }
    }

    stage('Test') {
      steps {
        sh 'cd /var/jenkins_home/workspace/FE-Instagram-CICD && npm run test'
        echo 'Run test successfully...'
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
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
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

    stage('Deploy to Minikube') {
      steps {
        script {
          sh "kubectl config use-context minikube"
          sh "kubectl apply -f /var/jenkins_home/workspace/FE-Instagram-CICD/deployment.yaml"
          //sh "kubectl rollout status deployment/$K8S_DEPLOYMENT --namespace=$K8S_NAMESPACE"
        }
      }
    }
  }
  
  post {
        always {
            // Clean up docker images
            cleanWs()
            sh "docker image prune -f"
        }
    }

}