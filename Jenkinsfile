pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Ansible') {
            steps {
                sh '''
                    echo "Checking Ansible..."
                    /usr/bin/ansible --version
                    /usr/bin/ansible-playbook --version
                '''
            }
        }

        stage('Check WAR') {
            steps {
                sh '''
                    echo "Checking WAR file..."
                    ls -lh /home/Project/DevOPS/LoginWebApp.war
                '''
            }
        }

        stage('Deploy WAR') {
            steps {
                ansiblePlaybook(
                    installation: 'ansible',
                    playbook: '/etc/ansible/playbooks/war_deploy.yml',
                    inventory: '/etc/ansible/playbooks/inventory',
                    extraVars: [
                        war_file: '/home/Project/DevOPS/LoginWebApp.war'
                    ],
                    colorized: true
                )
            }
        }
    }
}
