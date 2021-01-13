pipeline {
    agent any
    stages {
        stage ('Backend Build') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                }
                bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=06f1ba663de57dc2fb43ab6318ec7337718b5d8e -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn**,**/src/test/**,**/model/**,**Application.java "
            }
        }
        stage ('Quality Gate') {
            steps {
                sleep(5)
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortpipeline = true
                }
            }
        }
        stage ('Deploy Backend')
        steps {
            deploy adapters: [tomcat8(credentialsId: 'github_login', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target\\tasks-backend.war'
        }
    }
}