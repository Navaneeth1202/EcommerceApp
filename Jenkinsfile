pipeline {
    agent any
    
    tools {
        maven 'Maven'
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token-id')
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Msocial123/EcommerceApp.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarServer') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh 'cp target/*.war /var/lib/tomcat9/webapps/'
            }
        }
    }

    post {
        success {
            emailext (
                subject: "SUCCESS: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                body: "Build Successful at ${new Date()}",
                to: "your-email@gmail.com"
            )
        }
        failure {
            emailext (
                subject: "FAILED: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                body: "Build Failed at ${new Date()}",
                to: "your-email@gmail.com"
            )
        }
    }
}
