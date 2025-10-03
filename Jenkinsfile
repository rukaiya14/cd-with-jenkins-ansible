pipeline {
    agent any

    environment {
        WIN_USER = 'AnsibleDeploy'       // Your Windows username
        WIN_PASS = 'rukaiya'             // Your Windows password
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                bat """
                    docker run --rm -v "${WORKSPACE}:/ansible" ^
                    williamyeh/ansible:alpine3 ^
                    ansible-playbook -i /ansible/inventory.ini /ansible/ansible/site.yml ^
                    --extra-vars "ansible_user=%WIN_USER% ansible_password=%WIN_PASS%"
                """
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
