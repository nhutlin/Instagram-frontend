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
    //                     withKubeConfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJRGVSbTlRcEdHekF3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBMU1Ea3hPRFF6TWpKYUZ3MHpOREExTURjeE9EUTRNakphTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUUN4MitZQ1lxcU9tUTh4Z1NtajVHWjdIcXE4TENERnhGbmxrZ1dTS0g0Qk5MdkwyeDJpVEl1dWVSS2YKRzI4clNRUHBlaFZOTkpRcnZNWFdqWmIvc2tzcHdVeXpDOHNkWXFaUkFPaDhSM0k2a3RyWlNvY3IyMjh4WkJpTQpCNjhmdFprT3d4WGhMa2JXTmRlUmY2K3VKN1NyZ2xzOVRRbjZjcGJJeDltK1FEbkpPdGpUQytLYWxnL2dCZ1BkCms4SG5oU0E1OXVuclFQa0ovNEUrS3QzbmFzZDBSWmJDckxwQytXUjdIbmVWNk9IeGZnSWNFQXdqejd3MU00UFoKWFZGNmdqeHh2VUQwQXJKOVVvdy8rL2VwNjNvQ2wxNWhmM2I3eGMrTHRIK0JVYkxqa0VKR0JUMnVGUGJZcTRKZApnekRwMnRuVjlCZzFtUytUckFWNjRNWFdvQUhSQWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJUNWJ5dENpcUwyMmpjMngrbjhDTWdvN3RLaVlUQVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRQTJmd3NMZ0pQdApLbThzaDdlL0phNHI3Sm9CbFJGeXIrRFRyZVZyZ245aEd4OEUreGdCQnR0SHFVYkFna2srVlM5Q2Vra2ptUzQvCkNkTWFJVjhCYWpnS2ZGYTQ2SkROd1V6L09DWHMydWM0ZG1LdWpnOXpKL2tXdHJZVEU1Sm5jL2VCbkxFcVZUMHEKOUdHMVpSdnk2dGU3WkE1c3U3RkE5MTRZeHpGZDBrbDdXZSs0cm54YzhiK004ZExHUUtudElkN1Y3L09CYVRlUgpmT04yeHhZa3NWUlhaVVNYdXNhQjNBa2UyMnpVN2R5bkRvcHl6VWdJaWRNdTRjQVRwUHB3aFdQcmxJNVJnbGRKCnF2NU9aKzJ4VmJBVEIyRHhucVBub08wOG0wRlo1Wmo5L2tCYk43b0Q5K3czcEgrUW9LcnZRKzgyWUNsTjArYW8KTW5sU2NCMTRKQzVHCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K', clusterName: 'kubernetes', contextName: 'kubernetes-admin@kubernetes', credentialsId: 'k8sconfig2', namespace: 'default', restrictKubeConfigAccess: false, serverUrl: 'https://192.168.100.9:6443') {
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