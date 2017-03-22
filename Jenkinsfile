node {
	try {
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
	} catch (e) {
    	currentBuild.result = "FAILED"
    	notifyFailed()
    	throw e
  	}
}

def notifyFailed() {
  emailext (
      subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
      <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
      recipientProviders: [[$class: 'CulpritsRecipientProvider']]
  )

  mail bcc: '', body: '<p>Check console output at &QUOT;<a href=\'${env.BUILD_URL}\'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>"""', cc: '', from: '', replyTo: '', subject: 'Build Failed - ${env.JOB_NAME} [${env.BUILD_NUMBER}]', to: 'wkloc@pgs-soft.com'


  mail body: "FAILED BUILD. ${env.JOB_NAME} build error is here: ${env.BUILD_URL}", from: 'jenkins@pgs-soft.com', replyTo: 'wkloc@pgs-soft.com',
            subject: 'FAILED BUILD - ${env.JOB_NAME} build failed', to: 'wkloc@pgs-soft.com'

}