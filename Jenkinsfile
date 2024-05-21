pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building the code using Maven. Maven is used to compile and package the Java application."
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo "Running unit tests with JUnit and integration tests with Selenium. JUnit is used for unit testing, while Selenium handles integration tests to ensure components interact correctly."
            }
            post {
                success {
                    script {
                        emailext (
                            to: "juweria.amina@gmail.com",
                            subject: "SUCCESS: Unit and Integration Tests",
                            body: "The Unit and Integration Tests stage completed successfully. Please check the attached logs for details.",
                            attachLog: true
                        )
                    }
                }
                failure {
                    script {
                        emailext (
                            to: "juweria.amina@gmail.com",
                            subject: "FAILURE: Unit and Integration Tests",
                            body: "The Unit and Integration Tests stage failed. Please check the Jenkins dashboard and attached logs for details.",
                            attachLog: true
                        )
                    }
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo "Analysing code with SonarQube. SonarQube provides a detailed report of code quality and potential bugs."
            }
        }
        stage('Security Scan') {
            steps {
                echo "Performing security scan with OWASP ZAP. OWASP ZAP is used to detect security vulnerabilities in the application."
            }
            post {
                success {
                    script {
                        emailext (
                            to: "juweria.amina@gmail.com",
                            subject: "SUCCESS: Security Scan",
                            body: "The Security Scan stage completed successfully. Please check the attached logs for details.",
                            attachLog: true
                        )
                    }
                }
                failure {
                    script {
                        emailext (
                            to: "juweria.amina@gmail.com",
                            subject: "FAILURE: Security Scan",
                            body: "The Security Scan stage failed. Please check the Jenkins dashboard and attached logs for details.",
                            attachLog: true
                        )
                    }
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo "Deploying application to AWS EC2 staging instance. The AWS EC2 service is used to host the staging environment."
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests in the staging environment to validate production readiness. Integration tests ensure the app behaves as expected in a setup that mimics the production environment."
            }
        }
        stage('Deploy to Production') {
            steps {
                echo "Deploying application to AWS EC2 production instance. The production deployment uses AWS EC2, ensuring the application is available for end users."
            }
        }
    }
}