pipeline {


  agent any

  tools {
    maven 'Apache Maven 3.6.3'
  }
  stages {
    stage('build maven') {
      steps {
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ibtissame-oumahrir/devops-automation']])
        sh 'mvn clean install'
      }
    }
     stage('build image') {
      steps {
        script {
    
         // sh 'docker build -t ibtissameoumahrir/devops-integration .'
             sh "docker build -t ibtissameoumahrir/devops-integration:${env.BUILD_NUMBER} ."
             sh "docker tag ibtissameoumahrir/devops-integration:${env.BUILD_NUMBER} ibtissameoumahrir/devops-integration:latest"
           
        }
      }
    }
 stage('push image to hub'){
  steps{
    script{
   withCredentials([string(credentialsId: 'ibtissameoumahrir', variable: 'ibtissameoumahrir')]) {
            sh 'docker login -u ibtissameoumahrir -p ${ibtissameoumahrir}'
   }
        //sh 'docker push ibtissameoumahrir/devops-integration:latest'
        sh "docker push ibtissameoumahrir/devops-integration:${env.BUILD_NUMBER}"
        sh "docker push ibtissameoumahrir/devops-integration:latest"
}
}
}
stage('Run Docker Container') {
      steps {   
          sh 'docker run -d -p 8888:80 ibtissameoumahrir/devops-integration'
 } 
}
stage('deploy image to ku8'){
    steps{
        script{
            kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfigid')
    }
    }
}

}
}