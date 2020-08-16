def mvnHome
node('node') {
   stage('git checkout'){
      try {
         git branch: '', credentialsId: 'git-token', url: 'https://github.com/Pratima204/CI_CD_Integration.git'
      } catch(err) {
         sh "echo error in checkout"
      }
   }

   stage('maven test'){
      try{
         sh "$mvnHome/bin/mvn --version"
         sh "$mvnHome/bin/mvn clean test surefire-report:report"
      } catch(err) {
         sh "error in defining maven"
      }
      }
   }