pipeline {
	agent any
	tools {
        	jdk 'myjava'
        	maven 'mymaven'
    	}
	
	stages{
		stage('Checkout Code'){
			steps{
				checkout scm
			}
		}
	
		stage('Build'){
			steps{
				sh "mvn clean install -Dmaven.test.skip=true"
			}
		}

		stage('Test') { 
            		steps {
                		sh 'mvn test' 
            		}
            		post {
                		always {
                    			junit 'target/surefire-reports/*.xml' 
                		}
            		}
        	}
		
		stage('Archive Artifact'){
			steps{
				archiveArtifacts artifacts:'target/*.war'
			}
		}
	
		stage('deployment'){
			steps{
				deploy adapters: [tomcat9(url: 'http://localhost:8081/', 
                              		credentialsId: 'tomcatuser')], 
                     			war: 'target/*.war',
                     			contextPath: 'app'
			}
		
		}
	
		stage('Notification'){
			steps{
				emailext(
					subject: "Job Completed",
					body: "Jenkins pipeline job for maven build job completed",
					to: "devopseng129@gmail.com"
				)
			}
		}
	}
}
