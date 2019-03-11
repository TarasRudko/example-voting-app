pipeline {
  agent {
    node {
      label 'ubuntu-1604-aufs-stable'
    }
  }
  stages {
    stage('Build result') {
      steps {
        sh 'docker build -t tarasrudko/result ./result'
      }
    } 
    stage('Build vote') {
      steps {
        sh 'docker build -t tarasrudko/vote ./vote'
      }
    }
    stage('Build worker') {
      steps {
        sh 'docker build -t tarasrudko/worker ./worker'
      }
    }
    stage('Push result image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'docker_hub_login', url:'') {
          sh 'docker push tarasrudko/result'
        }
      }
    }
    stage('Push vote image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'docker_hub_login', url:'') {
          sh 'docker push tarasrudko/vote'
        }
      }
    }
    stage('Push worker image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'docker_hub_login', url:'') {
          sh 'docker push tarasrudko/worker'
        }
      }
    }
  }
}