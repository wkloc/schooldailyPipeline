node {
	try {
	   def mvnHome
	   stage('Checkout') { // for display purposes
	      // Get some code from a GitHub repository
	      git branch: 'develop', url: 'https://github.com/wkloc/schooldaily.git'
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
	         sh "'${mvnHome}/bin/mvn' clean install -DskipITs -Dspring.profiles.active=citest"
	      } else {
	         bat(/"${mvnHome}\bin\mvn" clean install -DskipITs -Dspring.profiles.active=citest/)
	      }
	   }
	   stage('Build - Integration Tests') {
	      // Run the maven build
	      if (isUnix()) {
	         sh "'${mvnHome}/bin/mvn' clean install -Dspring.profiles.active=citest"
	      } else {
	         bat(/"${mvnHome}\bin\mvn" clean install clean install -Dspring.profiles.active=citest/)
	      }
	   }
	   stage('Results') {
	      junit '**/target/surefire-reports/TEST-*.xml'
	      archive 'target/*.jar'
	   }
	} catch (e) {
    	currentBuild.result = "FAILED"
    	notifyFailed()
    	throw e
  	}
}

def notifyFailed() {
	final def RECIPIENTS = emailextrecipients([
        	[$class: 'DevelopersRecipientProvider'],
        	[$class: 'CulpritsRecipientProvider']
	])
	final def SUBJECT = "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - FAILED!"
	final def CONTENT = "Check console output at ${env.BUILD_URL} to view the results."
	if (RECIPIENTS != null && !RECIPIENTS.isEmpty()) {
	    mail to: RECIPIENTS, replyTo: "wkloc@pgs-soft.com", subject: SUBJECT, body: CONTENT
	} else {
	    mail to: "wkloc@pgs-soft.com", replyTo: "wkloc@pgs-soft.com", subject: SUBJECT, body: CONTENT
	}
}