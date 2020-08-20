pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/vdespa-collab/java-rest-api-calculator.git'
                sh './mvnw clean compile'
                // bat '.\\mvnw clean compile'
            }
        }
        stage('Test') {
            steps {
                sh './mvnw test'
                // bat '.\\mvnw test'
            }

            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
        stage('Publish') {
            steps {
                sh './mvnw package'
                // bat '.\\mvnw package'
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                    sh 'aws --version'
                    // bat 'aws --version'
                    sh 'aws iam get-user'
                    // bat 'aws iam get-user'
                    sh 'aws configure set region us-east-1'
                    sh 'aws s3 cp ./target/calculator-0.0.1-SNAPSHOT.jar s3://valentin-calculator-api-jars/calculator.jar'
                }
            }
        }
    }
}