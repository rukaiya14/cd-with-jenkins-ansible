pipeline {
    agent any

    environment {
        WIN_USER = 'AnsibleDeploy'
        WIN_PASS = 'rukaiya'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from Git repository..."
                checkout([$class: 'GitSCM', branches: [[name: 'main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/rukaiya14/cd-with-jenkins-ansible.git',
                        credentialsId: 'ansible-win-creds'
                    ]]
                ])
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo "Running Ansible Playbook to setup Tomcat 8..."
                bat """
                    docker run --rm ^
                        -v "%WORKSPACE%:/ansible" ^
                        williamyeh/ansible:alpine3 ^
                        ansible-playbook -i /ansible/inventory.ini /ansible/ansible/site.yml ^
                        --extra-vars "ansible_user=%WIN_USER% ansible_password=%WIN_PASS%"
                """
            }
        }

        stage('Post-Deployment Verification') {
            steps {
                echo "Deployment complete. You can add verification steps here."
            }
        }
    }

    post {
        success {
            echo "✅ Deployment finished successfully."
        }
        failure {
            echo "❌ Deployment failed. Check logs for errors."
        }
    }
}

