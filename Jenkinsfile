pipeline{
    agent any
    environment{
        APP_NAME = "reddit-clone-app"
    }
    stages{
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'github', url: 'https://github.com/Code-Cafe-IT/a-reddit-clone-gitops.git'
             }
         }
         stage("Update the Deployment Tags Image"){
            steps{
                //Tìm kiếm bất kỳ ký tự nào bao gồm ${APP_NAME} về sau thay thế bbằng /${APP_NAME}:${IMAGE_TAG}
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml 
                    cat deployment.yaml
                """
            }
         }
         stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "Code-Cafe-IT"
                    git config --global user.email "laminhduc2k1@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Code-Cafe-IT/a-reddit-clone-gitops.git main"
                }
            }
         }
    }
}