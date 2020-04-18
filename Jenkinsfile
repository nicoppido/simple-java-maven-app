pipeline {
    agent { 
        label 'master' 
    }
    environment {
        scannerHome = tool 'SonarQubeScanner'
        mvnHome = tool 'Maven'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
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
        stage('Code Analysis') {
             steps {
                    withSonarQubeEnv('SonarQube') {
                        sh "mvn sonar:sonar"
                    }
             }
        }
    }
}
