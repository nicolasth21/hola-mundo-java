pipeline {
    agent any
    environment {
        // Token de autenticación para Snyk (debes configurarlo en Jenkins o usar credenciales)
        SNYK_TOKEN = credentials('snyk-token-id')
        // URL de SonarQube (debes tenerlo configurado en Jenkins)
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
                sh 'npm install -g snyk || true'
                sh 'snyk auth $SNYK_TOKEN'
                sh 'snyk test || true'
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
                sh 'apt-get update && apt-get install -y wget'
                sh 'wget https://github.com/aquasecurity/trivy/releases/latest/download/trivy_0.50.1_Linux-64bit.deb'
                sh 'dpkg -i trivy_0.50.1_Linux-64bit.deb'
                sh 'trivy fs . || true'
            }
        }
        stage('Run') {
            steps {
                sh 'java -cp target/hola-mundo-java-1.0-SNAPSHOT.jar com.ejemplo.HelloWorld'
            }
        }
    }
}
