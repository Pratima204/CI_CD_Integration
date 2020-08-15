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

   stage('decker build and push'){
      try {
         sh "docker version"
         sh "docker build -t pratima1/archiveartifacts:newtag -f Dockerfile ."
         sh "docker run -p 8080:8080 -d pratima1/archiveartifacts:newtag"
         withDockerRegistry(credentialsId: 'docker-hub-registry') {
         sh "docker push pratima1/archiveartifacts:newtag"
         }
      }  catch(err){
           sh "echo error in deployment using docker"
      }
   }

   stage()

   stage('artifacts to s3'){
      try {
         withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'deploytos3', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            sh "aws s3 ls"
            sh "aws s3 mb s3://cloudyet-bucket-for-aws"
            sh "aws s3 cp addressbook_main/target/addressbook.war s3://cloudyet-bucket-for-aws"
         }
      }  catch(err) {
            sh "echo error in sending artifacts to s3"
      }
   }
} 