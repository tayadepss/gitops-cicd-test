pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "cicd-test-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'gitHub', url: 'https://github.com/Ashfaque-9x/gitops-register-app'
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
                   git config --global user.name "tayadepss"
                   git config --global user.email "tayadepss@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'gitHub', gitToolName: 'Default')]) {
                  sh "git push https://github.com/tayadepss/gitops-cicd-test main"
                }
            }
        }
      
    }
}
