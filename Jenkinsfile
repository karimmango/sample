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
    stage('Run production deployment') {
      when {
        branch 'main'
      }

      steps {
        build job: 'sample-deploy', parameters: [string(name: 'DEPLOY_TO', value: 'production'),
                                                 string(name: 'upstreamJobName', value: BRANCH_NAME)]
      }
    stage('Run QA deployment') {
      when {
        branch not 'main'
      }

      steps {
        build job: 'sample-deploy', parameters: [string(name: 'DEPLOY_TO', value: 'qa'),
                                                 string(name: 'upstreamJobName', value: BRANCH_NAME)]
      }
    
  }
}
  

//   stages {
//     stage('Copy artifact') {
//       steps {
//         copyArtifacts filter: 'sample', fingerprintArtifacts: true, projectName: 'sample_artifact', selector: lastSuccessful()
//       }
//     }
      
//     stage('Docker Login') {
//       steps {
//         withCredentials([usernamePassword(credentialsId: "docker-creds", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD' )]) {
//           sh 'docker login --username ${USERNAME} --password ${PASSWORD}'
//         }
//       }
//     }
//     stage('Docker build') {
//       steps {
//         sh 'docker build . --tag karimmango/devops:v1'
//       }
//     }
//     stage('Docker push') {
//       steps {
//        sh 'docker push karimmango/devops:v1'
//       }
//     }
//   }
