pipeline {
    agent any
    
    tools {
          maven 'Maven3.6.0'
          jdk 'java1.8.0'
    }
    stages {
        stage('Build') {
            steps {
                sh "mvn -B -DskipTests clean package"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                /*sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'*/
                
                sh "scp -o StrictHostKeyChecking=no -i //tmp//new.pem target//my-app-1.0-SNAPSHOT.jar centos@54.187.221.6://tmp//"
                sh "ssh -i //tmp//new.pem centos@54.187.221.6 java -jar //tmp//my-app-1.0-SNAPSHOT.jar"
            }
        }
    }
    
}
