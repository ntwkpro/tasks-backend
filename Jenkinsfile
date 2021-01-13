pipeline {
    agent any
    stages {
        stage ('Backend Build') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
    }
}