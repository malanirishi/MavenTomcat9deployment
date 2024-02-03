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
	
		stage('Deploy'){
			steps{
				deploy adapters: [tomcat9(url: 'http://localhost:8081/', 
                              		credentialsId: 'tomcatuser')], 
                     			war: 'target/*.war',
                     			contextPath: 'app'
			}
		
		}
	}
}
