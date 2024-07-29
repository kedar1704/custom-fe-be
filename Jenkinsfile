pipeline {
    agent any

    stages {
        stage('git pull') {
            steps {
                git branch: 'main', url: 'https://github.com/kedar1704/custom-fe-be.git'
            }
        }
        stage('Backend dockerfile build') {
            steps {
                sh 'cd /var/lib/jenkins/workspace/app_deploy/backend && docker build -t python-app .'
            }
        }
        stage('Frontend dockerfile build') {
            steps {
                sh 'cd /var/lib/jenkins/workspace/app_deploy/frontend && docker build -t node-app .'
            }
        }
        stage('Docker Push') {
              steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_creds', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                  sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
        }
      }
    }
}
