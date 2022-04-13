pipeline {
  agent any

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true, projectName: 'sample_artifact', selector: lastSuccessful()
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
    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: "docker-creds", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD' )]) {
          sh 'docker login --username ${USERNAME} --password ${PASSWORD}'
        }
      }
    }
    stage('Docker build') {
      steps {
        sh 'docker build . --tag karimmang/devops:v1'
      }
    }
    stage('Docker push') {
      steps {
       sh 'docker push karimmango/devops:v1'
      }
    }
  }
}
