pipeline {
    agent { label 'jenkinsnode1' } // Specify the agent label

    environment {
        ARGOCD_SERVER = '172.16.44.16:30282' // URL of your Argo CD server
        ARGOCD_AUTH_TOKEN = credentials('argocd-auth-token') // Use Jenkins credentials
    }

    stages {
        stage('Create ArgoCD Applications') {
            steps {
                script {
                    // Use a shell command to find all YAML files in the 'manifest' folder
                    sh '''
                    for app in manifests/*.yaml; do
                        if [ -f "$app" ]; then
                            echo "Creating ArgoCD application from: $app"
                            argocd app create -f "$app" --server ${ARGOCD_SERVER} --auth-token ${ARGOCD_AUTH_TOKEN}
                        else
                            echo "No YAML files found."
                        fi
                    done
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Applications created in Argo CD successfully!'
        }
        failure {
            echo 'There was a problem creating applications in Argo CD.'
        }
    }
}
