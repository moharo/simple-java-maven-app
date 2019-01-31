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
                sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
                
             }
        }
    
        stage('Artifactory configuration') {
  steps {
        rtServer (
		id: "ARTIFACTORY_SERVER",
		url: "http://54.214.99.187:8081/artifactory",
		credentialsId:"FROG_AF_CREDENTIALS"
		)

	rtMavenDeployer (
		id: "MAVEN_DEPLOYER",
		serverId: "ARTIFACTORY_SERVER",
		releaseRepo: "libs-release-local",
		snapshotRepo: "libs-snapshot-local"
		)

	rtMavenResolver (
		id: "MAVEN_RESOLVER",
		serverId: "ARTIFACTORY_SERVER",
		releaseRepo: "libs-release",
		snapshotRepo: "libs-snapshot"
		)
      }
 }

stage('Exec Maven') {
	steps {
		rtMavenRun(
			tool: "Maven3.6.0",
			pom: "pom.xml",
			goals: 'clean install',
			deployerId:"MAVEN_DEPLOYER",
			resolverId:"MAVEN_RESOLVER"
		)
	}
}

stage('Exec Repo') {
	steps {
		rtPublishBuildInfo (
		  serverId: "ARTIFACTORY_SERVER"
		)
	}
    }
}
    
}
