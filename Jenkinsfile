pipeline {
    agent {
        label "ansible_docker"
    }

    stages {
        stage('Get rep') {
            steps {
                git 'https://github.com/alsxs/example-playbook.git'
                sh 'ansible-vault decrypt secret --vault-password-file vault_pass'
                sh 'mkdir -p ~/.ssh/'
                sh 'mv ./secret ~/.ssh/id_rsa'
                sh 'chmod 400 ~/.ssh/id_rsa'
                sh 'echo -e "Host *\n   StrictHostKeyChecking no\n" >> ~/.ssh/config'
                sh 'ansible-galaxy install -r requirements.yml -p roles'
            }
        }
        stage('Run playbook') {
            steps {
                ansiColor('xterm') {
                    ansiblePlaybook(
                        playbook: 'site.yml',
                        inventory: 'inventory/prod.yml',
                        colorized: true)
                }
            }
        }
    }
}