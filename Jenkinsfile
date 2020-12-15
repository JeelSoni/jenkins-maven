pipeline {
    agent any 
    tools {
        maven "Maven 3"
    }
    stages{
       stage('Compile and Clean demo') { 
           steps { 
                sh "mvn --version"
                sh "mvn clean compile"
           }
        }
        stage('Sonarqube') { 
            environment { 
                scannerHome = tool 'SonarQubeScanner' 
            } 
            steps { 
                withSonarQubeEnv('sonarqube') { 
                    sh "${scannerHome}/bin/sonar-scanner sonar.projectKey=jenkins-maven sonar.sources=. sonar.java.binaries=**/target/classes" 
                } 
                timeout(time: 10, unit: 'MINUTES') { 
                    waitForQualityGate abortPipeline: true 
                } 
            } 
        } 
        stage('Test demo') { 
            steps {
                sh "mvn test "
            }
        }
        stage('Deploy') { 
            steps {
                sh "mvn package"
            }
        }
    }
}
