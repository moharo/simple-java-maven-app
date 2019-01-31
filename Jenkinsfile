pipeline {
    agent any
    
    tools {
          maven 'Maven3.6.0'
          jdk 'java1.8.0'
    }
    stages {
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
			pom: 'maven-example/pom.xml,
			goals: 'clean install',
			deployerId:"MAVEN_DEPLOYER",
			resolverId:"MAVEN_DEPLOYER"
		)
	}
}

stage('Exec Maven') {
	steps {
		rtPublishBuildInfo (
		  serverId: "ARTIFACTORY_SERVER"
		)
	}
    }
}
    
}
