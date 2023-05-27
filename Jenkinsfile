pipeline {


  agent any

  tools {
    maven 'Apache Maven 3.6.3'
  }
  stages {
    stage('build maven') {
      steps {
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ibtissame-oumahrir/devops-automation-']])
        sh 'mvn clean install'
      }
    }
     stage('build image') {
      steps {
        script {
    
          sh 'docker build -t ibtissameoumahrir/devops-integration .'
        }
      }
    }
 stage('push image to hub'){
  steps{
    script{
   withCredentials([string(credentialsId: 'ibtissameoumahrir', variable: 'ibtissameoumahrir')]) {
            sh 'docker login -u ibtissameoumahrir -p ${ibtissameoumahrir}'
   }
        sh 'docker push ibtissameoumahrir/devops-integration:latest'
}
}
}
}
}
