pipeline {
    agent any
stages {
  stage('Checkout SCM')
  {
    steps{
      checkout scm
      sh '''
      pwd 
      ls -ltr
      '''
        withwithCredentials([usernamePassword(credentialsId: 'ssh-12', keyfileVariable: 'SSH_KEY_FILE')])
        sh 'ssh -i "$(SSH_KEY_FILE)" -o StrictHostChecking=no ec2-user@13.126.52.93 "hostname -i"'
    
    }
  }
}
  
}
