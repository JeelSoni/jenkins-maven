pipeline {
    agent any 
    stages {
        stage('Compile and Clean demo') { 
            steps {

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
