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
                    kubeconfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJQmN2WmxJaDBLSkF3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBM01qa3hNVFEzTXpKYUZ3MHpOREEzTWpjeE1UVXlNekphTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUUNyMnl1V0Y0NElBUkRWK1V2MXdRbnhqL1RkdGJuZWtIWGt5eit2L001WlBjVUoxanJrYXVvOFAyL1kKejNZUGdNcWNITHVBSHdaczR6dzZaZXRTd3REc2pzTGFuOXZzcS9qUWZpK2dLZW1mRXpqa2hzMklhS1d5U3J3NgpLVnFWQzhDMW4rT1VPa3FmMFR1NkorN3BFRXVlTnlPd3V0THNmWE53M0V1akQrdG1HbW81a3VBbTh4YU5KckxHCms5TlNjUmNZK2RYZnVkUUN6NUpzaENOYUFndjVTK3B0a3lEMmxvU292V1pKeGwyZjFKYkhKaE1vOG9PMlpoRisKcDBMRHBtZ2dQQ2tGZ0VrelgxeG9ZblRnMVQxUG9KWU5hYWduS2J2bitaaGJENUc0bjlIUEJDUnVkUFFseVF0RgpvSnM4UTNweWQ5SEFaTVZSeVl2cjZwK0ltMkM1QWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJTdHdudHk1RkJwb21CUE8xUGpNOHNxeXhuNWpqQVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRQ2RtYXFjYkJaNwpOS1c2dXVZeXV3eGt4QkpaVXllOFBoZnZ1aXJ0d1hoUmVWaHdMcXZzNW9KY2ZaT2JNS0k2SGl5SXZNZHAvRXBXCkMzUkFzVFd5WldNWGZOc2VaZXA4SVRTaDg0eVhQdUduamY0dVlObmlqWW9CY1NjN3JGVTlFekZiMzdVMG9jY3oKRkJpN0hMendWZHFjd29SQVdiY2EzallFYk9Sc3o4RGRrU2FiekRaSkV4Umt1TktoRW9nWUJNemdGMG1tQnF4Ngp1NkJyOW1hVEVZSXNJWnBZR0ZnT1lhM0FtWHJPVUFQYnpRUk9NTHo5WXc5TkxHNFBBSklkM1dzai9ZRHpCVVNjCkZxRDFTLzBPVDJFL1ZhQkg1OWVUVS9CdWs4b1ZLeDlISGlDQWtJbzF2d1lOeDgzUU13dmVoZVVYcTMyNkdTVG8KeFhNWHpTUnI1MGxnCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K', credentialsId: 'kube_config', serverUrl: 'https://DF508AB5F6EE4C42AC093F0F55A601C2.gr7.ap-south-1.eks.amazonaws.com') {
                        sh 'kubectl get all -A'
}
            }
        }
    }
}
