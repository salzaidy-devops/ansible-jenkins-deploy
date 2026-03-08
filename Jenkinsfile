// pipeline {

//     agent any

//     stages {
//         stage('copy files to ansible server') {
//             steps {
//                 script {
//                     echo "Copying files to Ansible server..."
//                     // Add your file copy commands here
//                     sshagent(['ansible_server_key']) {
//                         sh 'scp -o StrictHostKeyChecking=no ansible/* root@104.131.175.93:/root'

//                         withCredentials([sshUserPrivateKey(credentialsId: 'aws-jenkins-key-pair', keyFileVariable: 'SSH_KEY_FILE', usernameVariable: 'SSH_USER')]) {
//                             sh "scp ${SSH_KEY_FILE} root@104.131.175.93:/root/dev-ec2-key.pem"
//                         }
//                     }
//                 }

//             }
//         }
//     }
// }

// pipeline {
//     agent any

//     stages {
//         stage('copy files to ansible server') {
//             steps {
//                 script {
//                     echo "Copying files to Ansible server..."

//                     sshagent(credentials: ['ansible_server_key']) {
//                         sh '''
//                             scp -o StrictHostKeyChecking=no ansible/* root@104.131.175.93:/root
//                         '''

//                         withCredentials([
//                             sshUserPrivateKey(
//                                 credentialsId: 'aws-jenkins-key-pair',
//                                 keyFileVariable: 'SSH_KEY_FILE',
//                                 usernameVariable: 'SSH_USER'
//                             )
//                         ]) {
//                             sh '''
//                                 scp -o StrictHostKeyChecking=no "$SSH_KEY_FILE" root@104.131.175.93:/root/dev-ec2-key.pem
//                             '''
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Run Ansible Playbook') {
//             steps {
//                 script {
//                     echo "Running Ansible Playbook on Ansible server to configure EC2 instances..."

//                     def remoteCommand = [:]
//                     remoteCommand.name = 'ansible_server'
//                     remoteCommand.host = '104.131.175.93'
//                     remoteCommand.allowAnyHosts = true

//                     withCredentials([
//                         sshUserPrivateKey(
//                             credentialsId: 'ansible_server_key',
//                             keyFileVariable: 'SSH_KEY_FILE',
//                             usernameVariable: 'SSH_USER'
//                         )
//                     ]) {
//                         remoteCommand.identityFile = SSH_KEY_FILE
//                         remoteCommand.username = SSH_USER

//                         sshCommand remote: remoteCommand, command: "ls -l"
//                     }
//                 }
//             }
//         }
//     }
// }
pipeline {
    agent any

    stages {
        stage('copy files to ansible server') {
            steps {
                script {
                    echo "Copying files to Ansible server..."

                    sshagent(credentials: ['ansible_server_key']) {
                        sh '''
                            scp -o StrictHostKeyChecking=no ansible/* root@104.131.175.93:/root
                        '''

                        withCredentials([
                            sshUserPrivateKey(
                                credentialsId: 'aws-jenkins-key-pair',
                                keyFileVariable: 'SSH_KEY_FILE',
                                usernameVariable: 'SSH_USER'
                            )
                        ]) {
                            sh '''
                                scp -o StrictHostKeyChecking=no "$SSH_KEY_FILE" root@104.131.175.93:/root/dev-ec2-key.pem
                            '''
                        }
                    }
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    echo "Running Ansible Playbook on Ansible server to configure EC2 instances..."

                    def remoteCommand = [:]
                    remoteCommand.name = 'ansible_server'
                    remoteCommand.host = '104.131.175.93'
                    remoteCommand.allowAnyHosts = true

                    withCredentials([
                        sshUserPrivateKey(
                            credentialsId: 'ansible_server_key',
                            keyFileVariable: 'SSH_KEY_FILE',
                            usernameVariable: 'SSH_USER'
                        )
                    ]) {
                        remoteCommand.identityFile = SSH_KEY_FILE
                        remoteCommand.user = SSH_USER

                        sshCommand remote: remoteCommand, command: 'ls -l /root'
                    }
                }
            }
        }
    }
}