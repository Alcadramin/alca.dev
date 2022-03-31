pipeline {
  agent {
    label 'amd64'
  }

  stages {
    stage ('Build') {
      steps {
        sh 'hugo -D -F -b "https://alca.dev" -d public'
      }
    }

    stage ('Deploy') {
      steps {
        sshagent(credentials: ['scarlet']) {
          sh 'ssh -v $SCARLET_URI'
          sh 'rsync -r "$WORKSPACE/public/" $ALCA.DEV_DEPLOY_PATH'
        }
      }
    }
  }
}