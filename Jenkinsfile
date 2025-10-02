pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Deploy with Ansible (via Docker)') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'ansible-win-creds', // Your Windows Credential ID
                    usernameVariable: 'WIN_USER',
                    passwordVariable: 'WIN_PASS'
                )]) {
                    bat """
                    echo 🚀 Running Ansible inside Docker...
                    docker run --rm ^
                        -v "${WORKSPACE}:/ansible" ^
                        williamyeh/ansible:alpine3 ^
                        ansible-playbook -i /ansible/inventory.ini /ansible/ansible/site.yml ^
                        --extra-vars "ansible_user=%WIN_USER% ansible_password=%WIN_PASS%"
                    """
                }
            }
        }

        stage('Post-Deployment Verification') {
            steps {
                echo "Deployment complete. Add verification steps here if needed."
            }
        }
    }

    post {
        success {
            echo "✅ Deployment finished successfully."
        }
        failure {
            echo "❌ Deployment failed. Please check logs."
        }
    }
}
