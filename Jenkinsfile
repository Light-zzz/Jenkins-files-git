pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
                sh '''
                    pwd 
                    ls -ltr
                '''
            }
        }

        stage('Access other VM') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-12', keyFileVariable: 'SSH_KEY_FILE', usernameVariable: 'SSH_USER')]) {
                    sh '''
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no $SSH_USER@13.126.52.93 "hostname -i"
                    '''
                }
            }
        }
    }
}
