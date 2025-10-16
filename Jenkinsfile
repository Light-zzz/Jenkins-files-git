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
                withCredentials([sshUserPrivateKey(credentialsId: 'WebApp', keyFileVariable: 'SSH_KEY_FILE',)]) {
                    sh '''
                        # Install nginx
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ubuntu@13.233.106.195 "sudo apt install nginx -y"

                        # Copy file
                        scp -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no webpage.html ubuntu@13.233.106.195:/tmp/index.html

                        # Move and restart nginx
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ubuntu@13.233.106.195 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html && sudo systemctl restart nginx"
                    '''
                }
            }
        }
    }
}

