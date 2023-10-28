pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "myapp-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Git Checkout") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/kohlidevops/gitops-register-app'
               }
        }

        stage("Docker Image Tag updation") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Deployment changes to K8 Repository") {
            steps {
                sh """
                   git config --global user.name "kohlidevops"
                   git config --global user.email "latchudevops1@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/kohlidevops/gitops-register-app main"
                }
            }
        }
      
    }
}
