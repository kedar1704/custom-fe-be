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
        stage('Docker Login') {
              steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_creds', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                  sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
               }
            }
        }
        stage('Docker Push') {
              steps {
                  sh "docker tag python-app kedar1704/python-app && docker push kedar1704/python-app"
                  sh "docker tag node-app kedar1704/node-app && docker push kedar1704/node-app"
          }
        }
        stage('get all') {
            steps {
                    sh 'kubectl get deployment'
                    sh 'kubectl get svc'
            }
        }
    }
}
