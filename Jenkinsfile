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
    }
}