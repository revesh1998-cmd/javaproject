pipeline {

    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/revesh1998-cmd/javaproject.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:5.0.0.4389:sonar'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                sh 'mvn deploy -DskipTests -s /var/lib/jenkins/.m2/settings.xml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline execution completed successfully.'
        }

        failure {
            echo 'Pipeline execution failed.'
        }

        always {
            echo 'Pipeline finished.'
        }
    }
}

