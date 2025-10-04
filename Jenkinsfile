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
                withCredentials([sshUserPrivateKey(credentialsId: 'App123', keyFileVariable: 'SSH_KEY_FILE',)]) {
                    sh '''
                        # Install nginx
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ec2-user@13.235.24.161 "sudo dnf install nginx -y"

                        # Copy file
                        scp -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no webpage.html ec2-user@13.235.24.161:/tmp/index.html

                        # Move and restart nginx
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ec2-user@13.235.24.161 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html && sudo systemctl restart nginx"
                    '''
                }
            }
        }
    }
}

