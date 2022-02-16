pipeline {

  agent any

  stages {
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("androshchuk/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    /*
      stage("Clear k8s resources") {
            steps {
              sh '''#!/bin/bash
                 sudo kubectl delete -f app.yaml
         '''
                
                }
            }
        */

    
    
    stage('Deploy App') {
      steps {
         sh '''#!/bin/bash
                 kubectl delete -f app.yaml'''
        script {
          kubernetesDeploy(configs: "app.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
