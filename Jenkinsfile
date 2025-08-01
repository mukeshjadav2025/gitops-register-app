pipeline {
    agent { label "jenkinsagent" }
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github-mukeshjadav2025', url: 'https://github.com/mukeshjadav2025/gitops-register-app'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "mukeshjadav0025"
                   git config --global user.email "mukeshjadav0025@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-mukeshjadav2025', gitToolName: 'Default')]) {
                  sh "git push https://github.com/mukeshjadav2025/gitops-register-app main"
                }
            }
        }
      
    }
}
