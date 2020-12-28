pipeline {
    agent{
        kubernetes {
		  //label 'maven'  // all your pods will be named with this prefix, followed by a unique id
		  idleMinutes 5  // how long the pod will live after no jobs have run on it
		  yamlFile 'pod.yaml'  // path to the pod definition relative to the root of our project 
		  defaultContainer 'node'  // define a default container if more than a few stages use it, will default to jnlp container
		}
    } 
    stages{
       stage('Compile and Clean demo') { 
            steps { 
                container('maven'){   
                    sh "mvn --version"
                    sh "mvn clean compile"
                }
            }
        }
        stage('Sonarqube') { 
            steps{
                container('node') { 
                    echo "Steps to execute SCA"
                    sh 'wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.3.0.1492-linux.zip'
                    sh 'unzip sonar-scanner-cli-3.3.0.1492-linux.zip'
    			    withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonarqube_access_token') {
    				    sh 'ls -l'
    				    sh 'sonar-scanner-3.3.0.1492-linux/bin/sonar-scanner -Dsonar.projectKey=jenkins-maven -Dsonar.sources=. -Dsonar.java.binaries=.'
    	    	    }
				    waitForQualityGate(abortPipeline: true, credentialsId: 'sonarqube_access_token')
			    }
            }
        } 
        stage('Test demo') { 
            steps {
                container('maven'){
                    sh "mvn test "
                }
            }
        }
        stage('Deploy') { 
            steps {
                container('maven'){
                    sh "mvn package"
                }
            }
        }
    }
}
