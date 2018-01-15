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
                stash(name: 'server', includes: '**/*.war')
            }
        }
        stage('Deploy') {
            agent {
                docker { 
                    image 'tomcat'
                    args '-u 0:0 -p 8085:8080'
                }
            }
            steps {
                unstash 'server'
                sh 'cp target/hello-world-war-1.0.0.war /usr/local/tomcat/webapps/hello-world-war-1.0.0.war && /usr/local/tomcat/bin/startup.sh && sleep 10 && wget -O - http://localhost:8080//hello-world-war-1.0.0'
                input(message: 'Deploy?', ok: 'Go!!')
            }
        }
    }
}
