node{
    stage("git code"){
        git url :"https://github.com/nagaraj1304/my-app.git", branch:'master'
    }
    stage("maven clean package"){
        def mvnHome = tool name:"mymaven",type:"maven"
        sh "${mvnHome}/bin/mvn clean package"
        sh "mv target/myweb*.war target/newapp.war"
    }
    stage("sonarqube test"){
        def mvnHome =  tool name: 'mymaven', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
    }
    stage("docker build image"){
        sh "docker build -t nagunani/myweb:0.0.5 ."
    }
    stage("docker login and push"){
       withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerpassword')]) {
            sh "docker login -u nagunani  -p ${dockerpassword}"
        }
        sh "docker push nagunani/myweb:0.0.5"
    }
    stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 18.223.43.214:1234"
   sh "docker tag nagunani/myweb:0.0.5 18.223.43.214:1234/nagu:1.0.0"
   sh 'docker push 18.223.43.214:1234/nagu:1.0.0'
   }
    stage("docker run"){
        sh "docker run -itd --name tomcatt -p 9070:8080 nagunani/myweb:0.0.5"
    }
}


