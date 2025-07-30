pipeline {
    agent {
        label 'jenkinsagent'
    }

    environment {
        APP_NAME = 'ci-cd-pipeline'   // Sai chính tả chỗ này
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs() // Viết đúng là cleanWs(), không phải cleanWS()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/NghiaNT05/deployment-e2epipeline.git'
            }
        }

        stage('Update the Deployment Tags') {
            steps {
                sh """
                cat deployment.yaml
                sed -i 's#${APP_NAME}:.*#${APP_NAME}:${IMAGE_TAG}#g' deployment.yaml
                cat deployment.yaml
                """
            }
        }

        stage('Push the changed deployment file to Git') {
            steps {
                sh """
                git config --global user.name "NghiaNT02"
                git config --global user.email "23521015@gm.uit.edu.vn"
                git add deployment.yaml || true
                git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                """

                    withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/NghiaNT05/deployment-e2epipeline.git main'
                }
            }
        }
    }
}
