node{
   stage('SCM Checkout'){
     git 'https://github.com/PandeeswariSubbaiya/Demo1.git'
   }
   stage('maven-buildstage'){
    def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
    stage('Build Docker Image'){
        sh 'docker build -t pandeeswari1814/demoimage .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerpass', variable: 'dockerPassword')]) {
   sh "docker login -u pandeeswari1814 -p ${dockerPassword}"
    }
   sh 'docker push pandeeswari1814/demoimage'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p Subbaiya@08  3.27.73.81:8082"
   sh "docker tag pandeeswari1814/demoimage 3.27.73.81:8082/damo:1.0.0"
   sh 'docker push 3.27.73.81:8082/damo:1.0.0'
   }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcatcontainer1 pandeeswari1814/demoimage' 
   }
}
