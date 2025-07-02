pipeline{
    agent {label "Jenkins-Agent"}
    environment{
        APP_NAME = "register-app-pipeline"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/zohaib9990/gitops-register-app.git'
            }
        }
        stage("Update the Deployment Tags"){
            steps{
                sh """ 
                cat deployment.yaml
                sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to git"){
            steps{
                sh """
                git config --global user.name "MSFaizi"
                git config --global user.email "kode2cloud@gmail.com"
                git add deployment.yaml
                git commit -m "Updated Deployment manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                    sh "git push https://github.com/zohaib9990/gitops-register-app.git main "
                }
            }
        }
    }
}
