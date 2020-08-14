def mvnHome

node('node'){
   stage('git checkout'){
      try {
       git credentialsId: 'git-token', url: 'https://github.com/Pratima204/CI_CD_Integration.git'
      } catch(err) {
         sh "echo error in checkout"
      }
   }

   stage('maven test'){
      try{
         mvnHome=tool 'maven-3.6.3'
         sh "$mvnHome/bin/mvn --version"
         sh "$mvnHome/bin/mvn clean test surfire-report:report"
      } catch(err) {
         sh "echo error in defining maven"
      }
   }    

   stage('test case and report'){
      try {
         echo "executing test cases"
         junit allowEmptyResults: true, testResults: 'addressbook_main/target/site/*.xml'
         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'addressbook_main/target/site', reportFiles: 'surefire-report.html', reportName: 'SurefireReportHTML', reportTitles: ''])
      
      } catch(err){
         throw err  
      }
   }

   stage('package and generate artifacts'){
      try { 
         sh "$mvnHome/bin/mvn clean package -DskipTests=true"
         archiveArtifacts allowEmptyArchive: true, artifacts: 'addressbook_main/target/**/*.war'
      } catch(err) {
         sh "echo error in packaging and generating artifacts"
      }
   }
} 