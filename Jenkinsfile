pipeline {

  environment {
    dockerimagename = "nhutlinh231/frontend-k8s"
    dockerImage = ""
    SONARQUBE_SCANNER_PARAMS = '-Dsonar.login=01578500b59e686815db1d8605822d07e78cf1db'
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
        sh 'cd /var/lib/jenkins/workspace/Github-FE-Instagram && npm install && npm run build'
        echo 'Run build successfully...'
      }
    }

    stage('Test') {
      steps {
        sh 'cd /var/lib/jenkins/workspace/Github-FE-Instagram && npm run test'
        echo 'Run test successfully...'
      }
    }

    // stage('SonarCloud Analysis') {
    //   steps {
    //     withSonarQubeEnv('sonarcloud') { 
    //       sh 'npm install -g sonarqube-scanner'
    //       sh 'sonar-scanner \
    //         -Dsonar.projectKey=nhutlin_Instagram-frontend \
    //         -Dsonar.organization=NhutLinh \
    //         -Dsonar.sources=. \
    //         -Dsonar.host.url=https://sonarcloud.io \
    //         -Dsonar.login=${env.SONARQUBE_SCANNER_PARAMS}'          
    //             }
    //         }
    //     }

    // stage('Quality Gate') {
    //   steps {
    //     waitForQualityGate abortPipeline: true
    //   }
    // }

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
            sh "docker image prune -f"
        }
    }
}
