pipeline {
    agent any 
    stages{
       stage('Compile and Clean demo') { 
            steps {
                def mvnHome = tool name: 'maven-3', type: 'maven' 
                sh "${mvnHome}/bin/mvn clean compile"
            }
        }
        stage('Sonarqube') {
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=jenkins-maven \
                    -Dsonar.sources=. "
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
