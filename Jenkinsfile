// FINAL JENKINSFILE: Deployment to Windows via Dockerized Ansible

pipeline {
    // The main pipeline agent runs on the Jenkins host (Windows) for checkout and simple tasks.
    agent any

    environment {
        // Placeholder environment variable
        BUILD_ID = "${env.JOB_NAME}-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                // Jenkins checks out the code to the workspace on the Windows host.
                echo 'Checking out source code...'
            }
        }

        stage('Build Application') {
            steps {
                // Placeholder for building your application artifact
                bat 'echo "Application build successful. Artifact ready."'
            }
        }

        stage('Deploy with Ansible (via Docker)') {
            // CRITICAL FIX: Define a Docker agent for this stage to guarantee a Linux environment.
            agent {
                docker {
                    image 'python:3.11-slim' // A minimal image where we can install Ansible
                    // We removed the complex 'args' to fix path conflicts on Windows.
                }
            }
            steps {
                // 1. Install necessary tools inside the temporary container
                sh 'pip install ansible'

                // 2. Checkout the repository again inside the container's workspace
                // This ensures files are accessible to the Linux shell.
                checkout scm
                
                // 3. Securely retrieve the Windows credentials
                withCredentials([usernamePassword(
                    credentialsId: 'ansible-win-creds', // Your Windows Credential ID
                    usernameVariable: 'WIN_USER',
                    passwordVariable: 'WIN_PASS'
                )]) {
                    // 4. Execute the Ansible playbook directly using the shell (sh)
                    // The variables are passed as extra-vars.
                    sh '''
                        echo "Starting WinRM deployment..."
                        ansible-playbook -i inventory.ini deploy.yml \
                        --extra-vars "ansible_user=$WIN_USER ansible_password=$WIN_PASS"
                    '''
                }
            }
        }

        stage('Post-Deployment Verification') {
            steps {
                echo "Deployment complete. Verification stages would follow."
            }
        }
    }
}
