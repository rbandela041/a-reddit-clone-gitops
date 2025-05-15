pipeline {
    agent any
    environment {
          APP_NAME = "reddit-clone-app"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'github', url: 'https://github.com/rbandela041/a-reddit-clone-gitops'
             }
         }
         stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i "s|^\(\s*image:\s*rbmihawk/reddit-clone-app:\).*|\1${IMAGE_TAG}|" deployment.yaml
                    cat deployment.yaml
                """
            }
         }
         stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "rbandela041"
                    git config --global user.email "rbandela1704@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/rbandela041/a-reddit-clone-gitops main"
                }
            }
         }
    }
}
