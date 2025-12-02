pipeline {
    agent {
        docker {
            image 'maven:3.8.6-openjdk-17'
        }
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
        stage('Run') {
            steps {
                sh 'java -cp target/hola-mundo-java-1.0-SNAPSHOT.jar com.ejemplo.HelloWorld'
            }
        }
    }
}
