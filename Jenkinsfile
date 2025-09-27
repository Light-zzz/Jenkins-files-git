pipeline {
    agent any
stages {
  stage(Checkout SCM)
  {
    steps{
      checkout scm
      sh '''
      pwd 
      ls -ltr
      '''
      withCredentials([ usernamePassword(credentialsId: 'ssh-12', ukeyfileVariable: 'SSH_KEY_FILE')])
      sh 'ssh -i "$(SSH_KEY_FILE)" -o StrictHostKeyChecking=no ec2-user@13.127.116.30 "hostname -i" '
    }
  }
}
  
}
