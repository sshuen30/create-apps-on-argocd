pipeline {
    agent { label 'jenkinsnode1' }

    environment {
        ARGOCD_SERVER = '172.16.44.16:30282' // URL of your Argo CD server
        ARGOCD_AUTH_TOKEN = credentials('argocd-auth-token') // Use Jenkins credentials
    }

    stages {
        stage('Create ArgoCD Applications') {
            steps {
                script {
                    // Find all application YAML files and create applications in Argo CD
                    def applicationFiles = findFiles(glob: '*.yaml') // Adjust the glob if needed
                    for (app in applicationFiles) {
                        sh """
                            argocd app create -f ${app} --server ${ARGOCD_SERVER} --auth-token ${ARGOCD_AUTH_TOKEN}
                        """
                    }
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
