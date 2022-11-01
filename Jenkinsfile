node{
    stage("clone code from github"){
        git url:"https://github.com/nagaraj1304/my-app.git", branch:"master"
    }
    stage(" maven clean package"){
        def mavenHome = tool name: "mymaven", type: "maven"
        sh "${mavenHome}/bin/mvn clean package"
        sh "mv target/myweb*.war target/newapp.war"
    }
    stage("docker build images"){
        sh "docker build -t nagunani/myweb:0.0.5 ."
    }
    stage("docker push & login"){
        withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerpassword')]) {
        sh "docker login -u nagunani -p ${dockerpassword}"
    }
        sh "docker push nagunani/myweb:0.0.5"
    }
    stage("docker run images"){
        sh "docker run -itd --name tomcattest -p 9070:8080 nagunani/myweb:0.0.5"
    }
}
 
