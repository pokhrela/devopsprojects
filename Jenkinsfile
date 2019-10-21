node{
 stage('Git Checkout'){
	git branch: 'dockercicd', url: 'https://github.com/prabhatpankaj/devopsprojects.git'  
 }
 stage('Maven Package'){
	def mvnHome = tool name: 'maven-3', type: 'maven'
	sh "${mvnHome}/bin/mvn clean package"
 	sh 'mv target/myweb*.war target/myweb.war'
 }
 
 stage('Build Docker Imager'){
   sh 'docker build -t prabhatiitbhu/myweb:0.0.1 .'
 }
 stage('Push to Docker Hub'){
	 withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerHubPwd')]) {
        sh "docker login -u prabhatiitbhu -p ${dockerHubPwd}"
     }
	 sh 'docker push prabhatiitbhu/myweb:0.0.1'
 }
 stage('Remove Previous Container'){
	try{
		def dockerRm = 'docker rm -f myweb'
		sshagent(['docker-dev']) {
			sh "ssh -o StrictHostKeyChecking=no ubuntu@34.230.45.59 ${dockerRm}"
		}
	}catch(error){
		//  do nothing if there is an exception
	}
 }
 stage('Deploy to Dev Environment'){
   def dockerRun = 'docker run -d -p 8080:8080 --name myweb prabhatiitbhu/myweb:0.0.1 '
   sshagent(['docker-dev']) {
    sh "ssh -o StrictHostKeyChecking=no ubuntu@34.230.45.59 ${dockerRun}"
   }

 }
 
}
