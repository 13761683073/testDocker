pipeline {
  agent none
  stages {
    stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps {
                sh 'mvn clean install -Dmaven.test.skip=true'
            }
      }
    stage('Build and publish docker image') {
      agent any
      steps {
        sh '''cd $WORKSPACE
        
        cp /root/.m2/repository/enjoy/springbootvip/1.0-SNAPSHOT/springbootvip-1.0-SNAPSHOT.jar \\
        /var/jenkins_home/workspace/testDocker_master/target/springbootvip-1.0-SNAPSHOT.jar
        
        docker build -t sprintbootvip:v$BUILD_NUMBER -f Dockerfile .
        
        docker stop sprintbootvip
        
        docker rm sprintbootvip
        
        docker run -d -p 8888:8080 --name sprintbootvip \\
         --ip="172.30.1.2" \\
         -v /root/jenkins/apache-tomcat-9.0.7/webapps/jenkins:/usr/tomcat/webapps/jenkins \\
         -v /root/.jenkins:/root/.jenkins sprintbootvip:v$BUILD_NUMBER'''
      }
    }

  }
}
