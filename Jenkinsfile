pipeline {
  environment {
    registry = "pratush43/dock"
    registryCredential = 'dockerhub'
    image = ''
    branch_name = $env.GIT_BRANCH
  }

  
  agent none
    stages {
        stage('Build') {
          agent {
    node{
    label 'micro'
    } 
  }
            steps {
           
              
              
sh 'echo "$branch_name"'

               
            
            }
        }
        stage("docker image"){
           agent any
      steps{
        script {
          echo "$env.GIT_BRANCH"
          unstash 'build'
         def dockerImage = docker.build registry + ":$BUILD_NUMBER"
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
        }
      }

     
  }
    }
    stage('Deploy image'){
      agent {
        node{
       label 'deployment' 
        }
      }
      steps{
        script{
        image = "$registry" + ":$BUILD_NUMBER"
            sh "docker run -d -p 8082:8081 '$image'"
      }
      }

    }
    }
    }
