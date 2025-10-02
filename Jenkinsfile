// Definitive Jenkinsfile for Ansible Deployment to Windows (via WSL)

pipeline {
    // Agent is set to 'any' since the actual execution happens via the
    // 'bat' step, which runs on the Windows host where WSL is installed.
    agent any

    environment {
        // Define your build tool environment variables here if needed
        // E.g., MVN_HOME = tool('Maven_3.8.6')
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Jenkins automatically clones the repository based on the job configuration
                echo 'Source code checked out successfully.'
            }
        }

        stage('Build Application') {
            steps {
                // Placeholder for building your application (e.g., a Java WAR file)
                // Use 'bat' for native Windows commands (like Maven, npm, etc.)
                bat 'echo "Application build steps would run here"'
                bat 'echo "Artifact created and ready for deployment"'
            }
        }

        stage('Deploy with Ansible (via WSL)') {
            steps {
                // This block securely retrieves the Windows credentials
                withCredentials([
                    usernamePassword(
                        // 🔑 This ID MUST match the 'Username with password' credential created in Jenkins
                        credentialsId: 'ansible-win-creds', 
                        usernameVariable: 'WIN_USER',
                        passwordVariable: 'WIN_PASS'
                    )
                ]) {
                    // CRITICAL STEP: Use 'bat' to execute the 'wsl' command.
                    // Credentials are passed as extra variables, overriding the inventory file.
                    bat "wsl ansible-playbook -i inventory.ini deploy.yml " +
                        "-e ansible_user=%WIN_USER% -e ansible_password=%WIN_PASS%"
                }
            }
        }

        stage('Post-Deployment Verification') {
            steps {
                // Optional: Add steps to verify deployment success
                bat 'echo "Deployment successful! Verification complete."'
            }
        }
    }
}
