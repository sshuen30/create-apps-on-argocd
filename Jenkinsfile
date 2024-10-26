pipeline {
    agent { label 'jenkinsnode1' }

    environment {
        ARGOCD_SERVER = '172.16.44.16:30282' // URL of your Argo CD server
        ARGOCD_AUTH_TOKEN = credentials('argcd-auth-token') // Use Jenkins credentials
    }

    stages {
        stage('Create ArgoCD Applications') {
            steps {
                script {
                    // List files in manifests directory for debugging
                    sh 'echo "Files in manifests directory:" && ls -la manifests/'

                    // Create ArgoCD applications for all YAML files
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
