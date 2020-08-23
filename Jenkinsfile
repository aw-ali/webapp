pipeline{
	agent any
	tools {
		maven 'Maven'
	}
	stages{
		stage ('initialize'){
			steps{
				sh '''
						echo "PATH = ${PATH}"
						echo "M2_HOME = ${M2_HOME}"		
				''' 
            }
		}
		
		stage ('Check-Git-Secrets') {
			steps {
				sh 'rm trufflehog || true'
				sh 'docker run gesellix/trufflehog --json https://github.com/aw-ali/webapp.git > trufflehog'
				sh 'cat trufflehog'
			}
		}
		
		stage('Build'){
			steps{
				sh 'mvn clean package'
			}
		}
		
		stage ('Deploy-To-Tomcat') {
			steps {
				sshagent(['tomcat']) {
					sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.129.244.141:/home/ubuntu/prob/apache-tomcat-8.5.57/webapps/webapp.war'
				}
			}
		}
	
	}

}