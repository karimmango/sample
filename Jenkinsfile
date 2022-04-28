pipeline {
  agent any

  parameters {
    choice choices: ['qa', 'production','cloud'], description: 'Select environment for deployment', name: 'DEPLOY_TO'

    string(name: 'upstreamJobName',
          defaultValue: '',
          description: 'The name of the job the triggering upstream build'
    )
  }



  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true,
          projectName: "sample-multibranch/${params.upstreamJobName}", selector: upstream()
      }
    }
    stage('Deliver') {
      steps {
        sshagent(['vagrant-private-key']) {
          sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i ${DEPLOY_TO}.ini playbook.yml'
        }
 
      }
    }
   
  }
}
