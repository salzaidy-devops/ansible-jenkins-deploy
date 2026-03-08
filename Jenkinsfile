pipeline {
    agent any

    stages {
        stage('copy files to ansible server') {
            steps {
                script {
                    echo "Copying files to Ansible server..."
                    // Add your file copy commands here
                    sshagent('ansible_server_key') {
                        sh 'scp -o StrictHostKeyChecking=no ansible/* root@104.131.175.93:/root'

                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-user', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                            sh "scp ${SSH_KEY} ansible/* ${SSH_USER}@104.131.175.93:/root/ssh_key.pem"
                        }
                    }
                }

            }
        }
    }
}