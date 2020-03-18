pipeline {
  agent any
  stages {
	stage('Unit Test') { 
      steps {
        bat 'mvn clean test -f maventest'
      }
    }
	stage('Upload Artifact') {
	  steps {
	  rtDownload (
			serverId: 'jenkins-artifactory-server',
			spec: '''{
				  "files": [
					{
					  "pattern": "libs-snapshot-local/com/mycompany/maventest/1.0.0-SNAPSHOT/",
					  "target": "snapshot-cai/"
					}
				 ]
			}''',
		 
			// Optional - Associate the uploaded files with the following custom build name and build number,
			// as build artifacts.
			// If not set, the files will be associated with the default build name and build number (i.e the
			// the Jenkins job name and number).
			buildName: 'CaiTest',
			buildNumber: '1'
		)
		rtUpload (
			serverId: 'jenkins-artifactory-server',
			spec: '''{
				  "files": [
					{
					  "pattern": "maventest/target/*maventest*.jar",
					  "target": "libs-snapshot-local/com/mycompany/maventest/1.0.0-SNAPSHOT/"
					}
				 ]
			}''',
		 
			// Optional - Associate the uploaded files with the following custom build name and build number,
			// as build artifacts.
			// If not set, the files will be associated with the default build name and build number (i.e the
			// the Jenkins job name and number).
			buildName: 'CaiTest',
			buildNumber: '1'
		)
	  }
	}
    stage('Deploy Standalone') { 
      steps {
        bat 'mvn clean package deploy -DmuleDeploy -f maventest'
      }
    }
  }
}