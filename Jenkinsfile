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
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Deploy') {
            agent {
                docker { 
                    image 'tomcat'
                    args '-u 0:0 -v /var/lib/jenkins/workspace/test-carlos/target/:/tmp -p 8080:8082'
                }
            }
            steps {
                sh 'cp /tmp/hello-world-war-1.0.0.war /usr/local/tomcat/webapps/ && sleep 5 && wget -O - http://localhost:8082//hello-world-war-1.0.0'
            }
        }
    }
}
