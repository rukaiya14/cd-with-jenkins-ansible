stage('Deploy with Ansible (via WSL)') {
    steps {
        // 1. CRITICAL: Retrieve the Windows credentials securely using the ID 'ansible-win-creds'
        withCredentials([usernamePassword(
            credentialsId: 'ansible-win-creds', // MUST match the ID in Jenkins Credentials
            usernameVariable: 'WIN_USER',
            passwordVariable: 'WIN_PASS'
        )]) {
            // 2. Execute Ansible via WSL, passing the variables to the command line.
            // %WIN_USER% and %WIN_PASS% are the username/password variables.
            bat "wsl ansible-playbook -i inventory.ini deploy.yml " +
                "-e ansible_user=%WIN_USER% -e ansible_password=%WIN_PASS%"
        }
    }
}
