node {
   // Mark the code checkout 'stage'....
   stage 'checkout'

   // Get some code from a GitHub repository
   git url: 'https://github.com/kesselborn/jenkinsfile'
   

   // Get the maven tool.
   // ** NOTE: This 'mvn' maven tool must be configured
   // **       in the global configuration.
   def mvnHome = tool 'maven-3.5.0'
   def java_Home = tool 'JDK 8'

   stage 'build'
   // set the version of the build artifact to the Jenkins BUILD_NUMBER so you can
   // map artifacts to Jenkins builds
   env.PATH = "${java_Home}/bin:${env.PATH}"
   sh "${mvnHome}/bin/mvn versions:set -DnewVersion=${env.BUILD_NUMBER}"
   sh "${mvnHome}/bin/mvn package"

   stage 'test'
   parallel 'test': {
     sh "${mvnHome}/bin/mvn test; sleep 2;"
   }, 'verify': {
     sh "${mvnHome}/bin/mvn verify; sleep 3"
   }

   stage 'archive'
   archive 'target/*.jar'
}


node {
   stage 'deploy Canary'
   sh 'echo "write your deploy code here"; sleep 5;'

   stage 'deploy Production'
   input 'Proceed?'
   sh 'echo "write your deploy code here"; sleep 6;'
   archive 'target/*.jar'
}

