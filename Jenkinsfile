pipeline {
  agent any

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true, projectName: 'sample', selector: lastSuccessful()
      }
    }
    stage('Deliver') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant_ssh", keyFileVariable: 'keyfile')]) {
            // making changes to test pipeline
            sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook --private-key=${keyfile} -i hosts.ini playbook.yml'
            // sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.3:'
        }
      }
    }

  }
}
