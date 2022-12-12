

pipeline {
  agent any
  stages {
    stage('Build'){
      steps{
      echo 'Download dependencies'
        withMaven(maven:"maven-387", publisherStrategy: 'EXPLICIT'){
          sh "pwd\n\
          cd api \n\
          ls -la\n\
          mvn install -DskipTests"
          }
        }
    }
    stage('Test'){
          steps{
              echo 'Test'
              sh "pwd\n\
              cd api\n\
              mvn test"
              }        
    }
    stage('Deploy'){
        steps{
            echo 'Deploy'
            }
          }        
    }
  }