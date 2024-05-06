pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'github-repo-url'
                git 'https://github.com/nhutlin/Instagram-frontend.git'
            }
        }
    }
}