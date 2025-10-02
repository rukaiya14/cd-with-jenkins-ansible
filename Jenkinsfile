pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pull the code from GitHub (includes Jenkinsfile, inventory, playbooks)
                checkout scm
            }
        }

        stage('Deploy with Ansible (via Docker)') {
            steps {
                // Inject Windows target credentials from Jenkins Credentials
                withCredentials([usernamePassword(
                    credentialsId: 'ansible-win-creds',   // Your Jenkins Credential ID
                    usernameVariable: 'WIN_USER',
                    passwordVariable: 'WIN_PASS'
                )]) {
                    // Run Ansible playbook inside a Docker container
                    sh '''
                        echo "🚀 Running Ansible inside Docker..."
                        docker run --rm \
                          -v $WORKSPACE:/ansible \
                          williamyeh/ansible:alpine3 \
                          ansible-playbook -i /ansible/inventory.ini /ansible/deploy.yml \
                          --extra-vars "ansible_user=$WIN_USER ansible_password=$WIN_PASS"
                    '''
                }
            }
        }

        stage('Post-Deployment Verification') {
            steps {
                echo "✅ Deployment complete. Add verification steps here if needed."
            }
        }
    }
}
