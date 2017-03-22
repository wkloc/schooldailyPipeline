node {
   def mvnHome
   stage('Checkout') { // for display purposes
      // Get some code from a GitHub repository
      git branch: 'testPlugins', url: 'https://github.com/wkloc/schooldaily.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M3'
   }
   stage('Compile') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' compile"
      } else {
         bat(/"${mvnHome}\bin\mvn" compile/)
      }
   }
   stage('Build - Unit Tests') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' clean install -DskipITs"
      } else {
         bat(/"${mvnHome}\bin\mvn" clean install -DskipITs/)
      }
   }
   stage('Build - Integration Tests') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' clean install -Dspring.profiles.active=ci"
      } else {
         bat(/"${mvnHome}\bin\mvn" clean install -Dspring.profiles.active=ci/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
}