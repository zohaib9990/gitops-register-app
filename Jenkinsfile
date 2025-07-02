pipeline {
    agent { label "Jenkins-Agent" }

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Image tag to update in deployment.yaml')
    }

    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "${params.IMAGE_TAG}"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/zohaib9990/gitops-register-app.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                echo "Before:"
                cat deployment.yaml

                sed -i 's|${APP_NAME}:.*|${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml

                echo "After:"
                cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to git") {
            steps {
                sh """
                git config --global user.name "zohaib"
                git config --global user.email "z7804166@gmail.com"
                git add deployment.yaml
                git commit -m "Updated deployment.yaml with image tag ${IMAGE_TAG}" || echo "No changes to commit"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/zohaib9990/gitops-register-app.git main"
                }
            }
        }
    }
}
