
def mvnHome

node ('node'){
   stage('git checkout'){
     try {
      git credentialsId: 'avi-tokenformasterpipekine', url: 'https://github.com/Pratima204/CI_CD_Integration.git'
   }    catch(err) {
           sh "echo error in checkout"
   }
}

   stage('maven test'){
      try {
         mvnHome=tool 'maven-3.6.3'
         sh "$mvnHome/bin/mvn --version"
         sh "$mvnHome/bin/mvn clean test surefire-report:report"
      } catch(err) {
         sh "echo error in defining maven"
      }
   }
}