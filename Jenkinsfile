pipeline {
    agent any

   stages {
        stage('GetCode') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', changelog: false, credentialsId: '72d80f05-bc1c-4179-b3e9-fe9a90459ecd', poll: false, url: 'https://github.com/hemanthmad/Maven-web-app.git'

            }
        }
        stage('Build'){
	        steps{
		        sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn clean package'
		    }
	    }
	    stage('SonarQube Analysis'){
	        steps{
		        withSonarQubeEnv('Sonar-Server-7.6'){
		        sh "/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn sonar:sonar"
		        }
	        }
	    }
	  stage('Deployment'){
		steps{
			script{
				def jenkinsWorkspace = "${WORKSPACE}"
				def tomcatWebapps = "/root/Hemanth_maven_learning/apache-tomcat-8.5.95/webapps"
				sh "chmod +r ${jenkinsWorkspace}/target/Demo-web-app.war"

        			// Copy the file using 'sh' step
        			sh "cp ${jenkinsWorkspace}/target/Demo-web-app.war ${tomcatWebapps}/"
			}
		}
	  }
    }
}
