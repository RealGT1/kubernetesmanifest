node {
    stage('Clone repository') {
        checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/RealGT1/kubernetesmanifest.git']]])
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    // Configure Git user
                    sh "git config user.email 'f.30.shreyasp@gmail.com'"
                    sh "git config user.name 'Shreyas P'"

                    // Update the client-deployment.yaml file with the new Docker tag
                    sh "sed -i 's+shreyas3557/client.*+shreyas3557/client:${params.DOCKERTAG}+g' client/client-deployment.yaml"
                    
                    // Update the server-deployment.yaml file with the new Docker tag
                    sh "sed -i 's+shreyas3557/server.*+shreyas3557/server:${params.DOCKERTAG}+g' server/server-deployment.yaml"
                    
                    // Display the updated files for verification
                    sh "cat client/client-deployment.yaml"
                    sh "cat server/server-deployment.yaml"
                    
                    // Commit and push the changes
                    sh "git add ."
                    sh "git commit -m 'Updated by Jenkins job updatemanifest: ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                }
            }
        }
    }
}
