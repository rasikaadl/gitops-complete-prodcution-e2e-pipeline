pipeline {
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "project-1-application"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
    
    
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github-access-token', url: 'https://github.com/rasikaadl/project-1-devops'
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
                    git config --global user.name "rasikaadl"
                    git config --global user.email "lfreez123@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-access-token', gitToolName: 'Default')]) {
                    sh "git push https://github.com/rasikaadl/project-1-devops main"
                }
            }
        }

    }

}
