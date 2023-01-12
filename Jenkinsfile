

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
            script{
                echo 'Test'
                sh "pwd\n\
                cd api\n\
                mvn test jacoco:report"
                }
                
                publishHTML([
                  allowMissing: true,
                  keepAll:true,
                  alwaysLinkToLastBuild: true,
                  reportDir: 'api/target/site/jacoco',
                  reportFiles: 'index.html',
                  reportName: 'Jacoco coverage HTML report'])
             }        
    }
    stage('Static code analysis'){
        steps{
        script{
            echo 'Static code analysis'
            sh "cd api\n\
              echo \"static code analysis finished\"\n\
              mvn -X clean pmd:pmd pmd:cpd findbugs:findbugs checkstyle:checkstyle \n\
              echo \"static code analysis finished\""
            }
            
            echo "Reading static analysis report"
            recordIssues enabledForFailure: true, failOnError: false, tool:checkStyle(pattern: "**/target/checkstyle-result.xml")
            recordIssues enabledForFailure: true, failOnError: false, tool:findBugs(pattern: "**/target/findbugs*.xml")
            recordIssues enabledForFailure: true, failOnError: false, tool:cpd(pattern: "**/target/cpd.xml")
            recordIssues enabledForFailure: true, failOnError: false, tool:pmdParser(pattern: "**/target/pmd.xml")
            }
          }        
    }
  }
// def runStaticCodeAnalysis(){
//   withMaven(maven: "maven-387", publisherStrategy: 'EXPLICIT'){
//   sh "cd api\n\
//   echo \"static code analysis finished\"\n\
//   mvn -X clean checkstyle:checkstyle\n\
//   echo \"static code analysis finished\""}
// }