pipeline {
    agent any

    environment {
        WIN_USER = "AnsibleDeploy"
        WIN_PASS = credentials('winrm-pass') // Stored in Jenkins credentials
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Deploy with Ansible (via Docker)') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'winrm-pass', 
                                                  usernameVariable: 'WIN_USER', 
                                                  passwordVariable: 'WIN_PASS')]) {
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
