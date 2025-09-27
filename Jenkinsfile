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
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-12', keyFileVariable: 'SSH_KEY_FILE',)]) {
                    sh '''
                        # Install nginx
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ec2-user@13.126.52.93 "sudo dnf install nginx -y"

                        # Copy file
                        scp -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no webpage.html ec2-user@13.126.52.93:/tmp/index.html

                        # Move and restart nginx
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ec2-user@13.126.52.93 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html && sudo systemctl restart nginx"
                    '''
                }
            }
        }
    }
}

