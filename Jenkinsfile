// FINAL JENKINSFILE: CI/CD Deployment to Windows via Ansible (WSL)

pipeline {
    // Agent is set to 'any' as the actual work is executed within the 'bat' command
    agent any

    environment {
        // Placeholder environment variable (required by Groovy syntax)
        BUILD_DATE = "${new Date().format('yyyyMMddHHmmss')}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Jenkins automatically clones the repository based on the job configuration
                echo "Repository checked out successfully at revision ${env.GIT_COMMIT}"
            }
        }

        stage('Build Application') {
            steps {
                // Placeholder for building your application artifact
                // Use 'bat' for native Windows commands (like Maven/npm)
                bat 'echo "Application build successful. Artifact ready."'
            }
        }

        stage('Deploy with Ansible (via WSL)') {
            steps {
                // 1. Securely retrieve the Windows credentials from Jenkins
                withCredentials([usernamePassword(
                    // 🔑 MUST match the ID of your 'Username with password' credential
                    credentialsId: 'ansible-win-creds', 
                    usernameVariable: 'WIN_USER',
                    passwordVariable: 'WIN_PASS'
                )]) {
                    // 2. CRITICAL FIX: Execute Ansible by calling 'wsl' via the 'bat' command.
                    // Credentials are passed as extra variables using Windows syntax (%VAR%).
                     bat "wsl -u rukaiya /usr/bin/ansible-playbook -i inventory.ini deploy.yml " +
                "-e ansible_user=%WIN_USER% -e ansible_password=%WIN_PASS%"
                }
            }
        }

        stage('Post-Deployment Verification') {
            steps {
                // Placeholder for running smoke tests or verification commands
                bat 'echo "Deployment verification complete. Check Windows server."'
            }
        }
    }
}
