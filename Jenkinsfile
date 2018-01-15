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
                    args '-u 0:0 -p 8085:8082'
                }
            }
            steps {
                unstash 'server'
                sh 'cp target/hello-world-war-1.0.0.war /usr/local/tomcat/webapps/hello-world-war-1.0.0.war && sleep 25 && wget -O - http://localhost:8082//hello-world-war-1.0.0'
                input(message: 'Deploy?', ok: 'Go!!')
            }
        }
    }
}
