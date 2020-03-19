pipeline {
  agent any
  environment {
	  groupId = readMavenPom().getGroupId().replaceAll('\\.','/')
	  artifactId = readMavenPom().getArtifactId()
	  artifactVersion = readMavenPom().getVersion()
	  artifactName = readMavenPom().getPackaging()
  }
  stages {
	stage('Unit Test') { 
      steps {
        bat 'mvn clean test'
      }
    }
	stage('Build Artifact') { 
      steps {
		bat 'mvn package'
      }
    }
	stage('Upload Artifact') {
	  steps {
	  //rtDownload (
			//serverId: 'jenkins-artifactory-server',
			//spec: '''{
			//	  "files": [
			//		{
			//		  "pattern": "libs-snapshot-local/com/mycompany/maventest/1.0.0-SNAPSHOT/",
			//		  "target": "snapshot-app/"
			//		}
			//	 ]
			//}''',
		 
			// Optional - Associate the uploaded files with the following custom build name and build number,
			// as build artifacts.
			// If not set, the files will be associated with the default build name and build number (i.e the
			// the Jenkins job name and number).
			//buildName: 'AppTest',
			//buildNumber: '1'
		//)
		rtUpload (
			serverId: 'jenkins-artifactory-server',
			spec: '''{
				  "files": [
					{
					  //"pattern": "target/maventest*.jar",
					  "pattern": "target/${artifactId}-${artifactVersion}-${artifactName}.jar",
					  //"target": "libs-snapshot-local/com/mycompany/maventest/1.0.0-SNAPSHOT/"
					  "target": "libs-snapshot-local/${groupId}/${artifactId}/${artifactVersion}/"
					}
				 ]
			}''',
		 
			// Optional - Associate the uploaded files with the following custom build name and build number,
			// as build artifacts.
			// If not set, the files will be associated with the default build name and build number (i.e the
			// the Jenkins job name and number).
			buildName: 'AppTest',
			buildNumber: '1'
		)
	  }
	}
    stage('Deploy Standalone') { 
      steps {
        bat 'mvn clean package deploy -DmuleDeploy'
      }
    }
  }
}
