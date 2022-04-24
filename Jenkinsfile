pipeline {
  agent any
  parameters {
    choice choices: ['qa', 'production','staging'], description: 'Select environment for deployment', name: 'DEPLOY_TO'
    string(name: 'upstreamJobName',
          defaultValue: 'main',
          description: 'The name of the job the triggering upstream build'
    )
  }



  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true,
          projectName: "sample_multibranch/${params.upstreamJobName}", selector: lastSuccessful()
      }
    }
    stage('Deliver') {
      steps {
        sshagent(['vagrant_ssh']) {
          sh 'ansible-playbook -i ${DEPLOY_TO}.ini playbook.yml'
        }
      }
    }
  }
}
