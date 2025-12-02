pipeline {
    agent any
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
        stage('Security Scan') {
            steps {
                // Ejemplo: análisis con SonarQube (si está configurado en Jenkins)
                sh 'echo "Ejecutando escaneo de seguridad con SonarQube..."'
            }
        }
        stage('Run') {
            steps {
                sh 'java -cp target/hola-mundo-java-1.0-SNAPSHOT.jar com.ejemplo.HelloWorld'
            }
        }
    }
}