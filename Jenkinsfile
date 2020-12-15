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
