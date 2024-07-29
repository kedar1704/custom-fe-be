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
                kubeconfig(credentialsId: 'kube_config', serverUrl: 'https://DF508AB5F6EE4C42AC093F0F55A601C2.gr7.ap-south-1.eks.amazonaws.com') {
                    sh 'kubectl get deployment'
                    sh 'kubectl get svc'
                }
            }
        }
    }
}
