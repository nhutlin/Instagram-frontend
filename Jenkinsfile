pipeline {

  environment {
    dockerimagename = "nhutlinh231/frontend-k8s"
    dockerImage = ""
  }

  agent any

  tools {
    nodejs '20.13.1'
  }

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/nhutlin/Instagram-frontend.git'
      }
    }

    stage('Build') {
      steps {
        sh 'npm -v'
        sh 'cd /var/lib/jenkins/workspace/FE-Instagram-CICD && npm install && npm run build'
        echo 'Run build successfully...'
      }
    }

    stage('Test') {
      steps {
        sh 'cd /var/lib/jenkins/workspace/FE-Instagram-CICD && npm run test'
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

    // stage('Deploy to K8s'){
    //         steps{
    //             script{
    //                 dir('Kubernetes') {
    //                     withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: 'kubernetes-admin@kubernetes', credentialsId: 'k8sconfig2', namespace: 'default', restrictKubeConfigAccess: false, serverUrl: 'https://192.168.100.9:6443') {
    //                             sh 'kubectl apply -f /var/lib/jenkins/workspace/FE-Instagram-CICD/deployment.yml'
    //                     }   
    //                 }
    //             }
    //         }
    //     }

    // stage('Deploy to K8s'){
    //   steps{
    //     script{
    //       dir('Kubernetes') {
    //         withKubeConfig(caCertificate: '', clusterName: 'minikube', contextName: 'minikube', credentialsId: 'minikubeconfig', namespace: 'default', restrictKubeConfigAccess: false, serverUrl: 'https://192.168.49.2:8443/') {
    //           sh 'kubectl apply -f /var/lib/jenkins/workspace/FE-Instagram-CICD/deployment.yml'
    //         } 
    //       }
    //     }
    //   }
    // }
  }

  post {
        always {
            // Clean up docker images
            cleanWs()
            sh "docker image prune -f"
        }
    }
}
