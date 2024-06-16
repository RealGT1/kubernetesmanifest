pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Update GIT') {
            steps {
                script {
                    // Retrieve credentials from Jenkins credentials store
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        // Configure Git user
                        sh "git config user.email 'samarthpuru3557@gmail.com'"
                        sh "git config user.name 'RealGT1'"

                        // Update deployment.yaml with DOCKERTAG
                        sh "cat deployment.yaml"
                        sh "sed -i \"s+shreyas3557/test.*+shreyas3557/test:${DOCKERTAG}+g\" deployment.yaml"
                        sh "cat deployment.yaml"

                        // Stage, commit, and push changes
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"

                        // Fetch and merge remote changes before pushing
                        sh "git fetch origin main"
                        sh "git merge origin/main"

                        // Push changes to GitHub repository
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                    }
                }
            }
        }
    }
}
