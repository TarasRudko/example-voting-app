pipeline {
  agent any
  
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
    stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
               // input 'Deploy to Production?'
               // milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/redis-deployment.yaml', 
                    enableConfigSubstitution: true
                )
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/redis-service.yaml', 
                    enableConfigSubstitution: true
                )

                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/db-deployment.yaml', 
                    enableConfigSubstitution: true
                )

                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/db-service.yaml', 
                    enableConfigSubstitution: true
                )
                 kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/vote-deployment.yaml', 
                    enableConfigSubstitution: true
                )

                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/vote-service.yaml', 
                    enableConfigSubstitution: true
                )

                 kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/result-deployment.yaml', 
                    enableConfigSubstitution: true
                )

                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/result-service.yaml', 
                    enableConfigSubstitution: true
                )
                 kubernetesDeploy(
                    kubeconfigId: 'kubeconfig', 
                    configs: './k8s-specifications/worker-deployment.yaml', 
                    enableConfigSubstitution: true
                )


            }
        }
  }
}