node {
    def app

    stage('Clone Repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github-secret-token', 
                                                  passwordVariable: 'GIT_PASSWORD', 
                                                  usernameVariable: 'GIT_USERNAME')]) {
                    // Configure Git user details
                    sh "git config user.email prashanth.kotamraju@gmail.com"
                    sh "git config user.name prashkotam"

                    // Display and modify the YAML file
                    sh "cat vote-ui-deployment.yaml"
                    sh """
                        sed -i 's+prashkotam/vote.*+prashkotam/vote:${DOCKERTAG}+g' vote-ui-deployment.yaml
                    """
                    sh "cat vote-ui-deployment.yaml"

                    // Commit and push changes
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job deployment: ${env.BUILD_NUMBER}'"
                    sh """
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/vote-deploy.git HEAD:master
                    """
                }
            }
        }
    }
}
