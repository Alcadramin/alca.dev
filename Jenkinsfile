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
        echo "${ALCA_DEPLOY_PATH}"
        sshagent(credentials: ['scarlet']) {
          sh 'rsync -avz -e "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null" --progress -r "$WORKSPACE/public/" ${SCARLET_URI}:${ALCA_DEPLOY_PATH}'
        }
      }
    }
  }
}