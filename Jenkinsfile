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
          projectName: "sample_multibranch/${params.upstreamJobName}", selector: lastSuccessful()
      }
    }
    stage('Deliver') {
      steps {
        sshagent(['cloud']) {
          sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i cloud.ini playbook.yml'
        }
 
      }
    }
   
  }
}
