pipeline {
    agent {
        label 'maven'
    }
    stages {
        stage('Build') {
            steps {
                sh "mvn -B -DskipTests clean package"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
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
                    withCredentials(bindings: [string(credentialsId: 'SonarQubeToken', variable: 'SONARQUBE_TOKEN')]) {
                        sh "mvn sonar:sonar -Dsonar.projectKey='example-java' -Dsonar.projectVersion='1.0.0' -Dsonar.login=${SONARQUBE_TOKEN} -Dsonar.host.url='http://172.18.0.4:9000/' -Dsonar.sources='./src/main/java/com/mycompany/app/' -Dsonar.sonar.tests='./src/test/java/com/mycompany/app/'"                    }
                }
            }
        }
    }
}
