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
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ubuntu@65.0.185.231 "sudo apt update && sudo apt install nginx -y"

                        # Copy file
                        scp -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no webpage.html ubuntu@65.0.185.231:/tmp/index.html

                        # Move and restart nginx
                        ssh -i "$SSH_KEY_FILE" -o StrictHostKeyChecking=no ubuntu@65.0.185.231 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html && sudo systemctl restart nginx"
                    '''
                }
            }
        }
    }
}

// Advance Pipeline scripts
// pipeline {
//     agent any

//     stages {
//         stage('Checkout SCM') {
//             steps {
//                 checkout scm
//                 sh '''
//                     echo "Current Directory:"
//                     pwd 
//                     echo "Files:"
//                     ls -ltr

//                     if [ ! -f webpage.html ]; then
//                         echo "ERROR: webpage.html not found"
//                         exit 1
//                     fi
//                 '''
//             }
//         }

//         stage('Access other VM') {
//             steps {
//                 withCredentials([sshUserPrivateKey(credentialsId: 'WebApp', keyFileVariable: 'SSH_KEY_FILE')]) {
//                     script {
//                         try {
//                             sh '''
//                                 ssh -i "$SSH_KEY_FILE" -o IdentitiesOnly=yes -o StrictHostKeyChecking=no ubuntu@65.0.185.231 "sudo apt update && sudo apt install nginx -y"

//                                 scp -i "$SSH_KEY_FILE" -o IdentitiesOnly=yes -o StrictHostKeyChecking=no webpage.html ubuntu@65.0.185.231:/tmp/index.html

//                                 ssh -i "$SSH_KEY_FILE" -o IdentitiesOnly=yes -o StrictHostKeyChecking=no ubuntu@65.0.185.231 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html && sudo systemctl restart nginx"
//                             '''
//                         } catch (err) {
//                             echo "ERROR: ${err}"
//                             error("Deployment failed.")
//                         }
//                     }
//                 }
//             }
//         }
//     }
// }
