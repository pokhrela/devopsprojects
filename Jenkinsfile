node{
	stage('SCM Checkout'){
		git branch: 'wartomcat', url: 'https://github.com/pokhrela/devopsprojects.git'
	}
	stage('Compile-Package'){
		def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn package"
	}
	stage('Deploy to Tomcat'){
		sshagent(['tomcat-dev']) {
		sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@3.93.63.179:/opt/tomcat9/webapps/'
	}
	}
}
