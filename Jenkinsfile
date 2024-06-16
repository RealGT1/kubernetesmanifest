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
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh "git config user.email samarthpuru3557@gmail.com"
                            sh "git config user.name RealGT1"
                            sh "cat deployment.yaml"
                            sh "sed -i 's+shreyas3557/test.*+shreyas3557/test:${DOCKERTAG}+g' deployment.yaml"
                            sh "cat deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"

                            // Fetch remote changes
                            sh "git fetch origin"

                            // Attempt to merge changes if needed
                            sh "git merge origin/main"

                            // Push changes
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
