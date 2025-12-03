pipeline {
    agent any
    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Security Scan - Snyk') {
            steps {
                withCredentials([string(credentialsId: 'snyk-token-id', variable: 'SNYK_TOKEN')]) {
                    sh '''
                        snyk auth $SNYK_TOKEN
                        snyk test || true
                    '''
                }
            }
        }
        stage('Static Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=hola-mundo-java -Dsonar.host.url=http://localhost:9000 -Dsonar.login=squ_ad3650ad13e04a6a56d94dfef38e5fccccbdb30a'


                }
            }
        }
        stage('Image Scan - Trivy') {
            steps {
                sh '''
                    trivy fs . || true
                '''
            }
        }
        stage('Run') {
            steps {
                sh 'java -cp target/hola-mundo-java-1.0-SNAPSHOT.jar com.ejemplo.HelloWorld'
            }
        }
    }
}
