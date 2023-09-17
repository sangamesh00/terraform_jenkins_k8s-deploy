pipeline {
 agent any
 options {
    timeout(time: 1, unit: 'HOURS')
 }
 triggers {
    pollSCM ('* * * * *')
    
 }
 stages {
    stage ('vcs'){
     steps {
        git url: 'https://github.com/sangamesh00/test00.git',
            branch: 'master'
     }
    }
    stage(build) {
      steps{
         sh script: 'docker image build -t testimage:latest .'
         sh script: 'docker image tag testimage:latest gheware/test01image:latest'
         sh script: 'docker image push gheware/test01image:latest'
      }
    
    }
    stage(k8sterrafaorm){
      steps{
         sh script: 'cd terraform && terraform init && terraform apply -auto-approve'
         
      }
    }
    stage(kubectl){
      steps {
         sh script: 'aws eks --region $(terraform output -raw region) update-kubeconfig \
                                --name $(terraform output -raw cluster_name)'

      }
    }


 }
}