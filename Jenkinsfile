pipeline {
    agent any

    tools {
        maven 'Maven 3.8.8' // You should configure this name in Jenkins > Global Tool Configuration
    }

    environment {
        SONARQUBE = 'MySonarQube'  // This must match the name you gave in Jenkins > Configure System > SonarQube servers
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/arjunpilakkal/Java-Calculator.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline successful!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
