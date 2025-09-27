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
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ec2-user@13.126.52.93 "sudo dnf install nginx -y"
                        ls -lrt
                        scp -i "$SSH_KEYF_ILE" webpage.html ec2-user@13.126.52.93:/usr/share/nginx/html/
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ec2-user@13.126.52.93 "sudo systemctl restart nginx"
                    '''
                }
            }
        }
    }
}
