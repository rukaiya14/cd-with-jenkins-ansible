pipeline {
    agent any

    environment {
        // Optional global environment variables can go here
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy with Ansible (via Docker)') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'ansible-win-creds',   // Your Windows target creds for WinRM
                    usernameVariable: 'WIN_USER',
                    passwordVariable: 'WIN_PASS'
                )]) {
                    bat '''
                        echo 🚀 Running Ansible inside Docker...
                        docker run --rm ^
                          -v "%WORKSPACE%:/ansible" ^
                          williamyeh/ansible:alpine3 ^
                          ansible-playbook -i /ansible/inventory.ini /ansible/deploy.yml ^
                          --extra-vars "ansible_user=%WIN_USER% ansible_password=%WIN_PASS%"
                    '''
                }
            }
        }

        stage('Post-Deployment Verification') {
            steps {
                echo "✅ Deployment complete. You can add verification steps here."
            }
        }
    }

    post {
        always {
            echo "Pipeline finished (success or failure)."
        }
        success {
            echo "🎉 Deployment succeeded!"
        }
        failure {
            echo "❌ Deployment failed. Please check logs."
        }
    }
}
