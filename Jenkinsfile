pipeline {
    agent any
    environment {
        SONAR_HOST_URL = 'http://localhost:9000' // Ajusta si tu SonarQube está en otro puerto/IP
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
                        apt-get update && apt-get install -y npm
                        npm install -g snyk
                        export PATH=$PATH:/usr/local/bin
                        snyk auth $SNYK_TOKEN
                        snyk test || true
                    '''
                }
            }
        }
        stage('Static Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=hola-mundo-java -Dsonar.host.url=$SONAR_HOST_URL'
                }
            }
        }
        stage('Image Scan - Trivy') {
            steps {
                sh '''
                    apt-get update && apt-get install -y wget
                    wget https://github.com/aquasecurity/trivy/releases/latest/download/trivy_0.50.1_Linux-64bit.deb
                    dpkg -i trivy_0.50.1_Linux-64bit.deb
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
