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
	
		stage('Compile'){
			steps{
				sh 'mvn compile'
				//sh "mvn clean install -Dmaven.test.skip=true"
			}
		}

		stage('Code Review') {
			steps {
				sh 'mvn pmd:pmd'
			}
		}

		stage('Unit Test') { 
            		steps {
                		sh 'mvn test' 
            		}
            		post {
                		always {
                    			junit 'target/surefire-reports/*.xml' 
                		}
            		}
        	}

		stage('Package') {
			steps {
				sh 'mvn clean package'
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
					to: "malanirishi@gmail.com"
				)
			}
		}
	}
}
