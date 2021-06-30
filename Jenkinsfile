pipeline {

  environment {
  imagename="satyabharath99/petclinic_pipeline"
  registryCredential="dbb113c3-8945-4ab2-84c6-8219820fae2f"
  dockerImage=''
  }
  
  agent any
  
  stages{
    stage('git'){
      steps{
        echo 'Cloning Repo...'
        git branch: 'main', url:'https://github.com/satyabharath99/spring-petclinic.git'
        
        echo 'Repo cloned successfully'
      }
    }
    
    stage('build'){
      steps{
        echo 'building the application...'
        sh './mvnw package'
      }
    }
    
    stage('deploy'){
      steps{
        echo 'Deploying the application...'
        
        script{
          dockerImage = docker.build imagename
        }
      }
    }
    
    stage('push'){
      steps{
        echo 'pushing image to dockerhub...'
        script{
          docker.withRegistry('', registryCredential){
            dockerImage.push("$BUILD_NUMBER")
            dockerImage.push('latest')
          }
        }
      }
    }
    
    stage('Removing unused docker image'){
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:latest"
      }
    }
    
    
  }
}
