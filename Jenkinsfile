pipeline {
  environment {
    registry = "pratush43/dock"
    registryCredential = 'dockerhub'
    image = ''
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
           
            BRANCH_NAME = "${GIT_BRANCH.split('/').size() > 1 ? GIT_BRANCH.split('/')[1..-1].join('/') : GIT_BRANCH}"
echo $BRANCH_NAME

                sh 'dotnet build'
              sh ' ls -lrt && pwd'
              archiveArtifacts artifacts: 'bin/Debug/net6.0/*.dll'
              stash includes: 'bin/Debug/net6.0/*.dll', name: 'build', useDefaultExcludes: false
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
