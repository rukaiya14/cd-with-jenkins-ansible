pipeline {
    agent any

    stages {
        stage('Checkout Source Code') {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                bat '''
                    echo "Application build successful. Artifact ready."
                '''
            }
        }

        stage('Deploy with Ansible (via Docker)') {
            steps {
                // 3. Securely retrieve the Windows credentials
                withCredentials([usernamePassword(
                    credentialsId: 'ansible-win-creds', // Your Windows Credential ID in Jenkins
                    usernameVariable: 'WIN_USER',
                    passwordVariable: 'WIN_PASS'
                )]) {
                    // 4. Run ansible-playbook inside a Docker container
                    bat '''
                        echo Starting WinRM deployment...
                        docker run --rm ^
                          -v %cd%:/ansible ^
                          -w /ansible ^
                          williamyeh/ansible:alpine3 ansible-playbook -i inventory.ini deploy.yml ^
                          --extra-vars "ansible_user=%WIN_USER% ansible_password=%WIN_PASS%"
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
