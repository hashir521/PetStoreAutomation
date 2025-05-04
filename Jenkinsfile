pipeline {
    agent any

    tools {
        maven 'mymaven'     // Must match the name you gave in Jenkins Global Tool Config
        jdk 'myjava'        // Must match the configured JDK name in Jenkins
    }

    stages {
        stage('Workspace Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Allure Report') {
            steps {
                // Clean old allure results (especially helpful on Windows)
                bat 'if exist target\\allure-results rmdir /s /q target\\allure-results'

                // Optional: regenerate results (if needed)
                // bat 'mvn allure:report'

                allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }

        success {
            echo 'Build succeeded!'
        }

        failure {
            echo 'Build failed. Please check logs.'
        }

        unstable {
            echo 'Build marked as UNSTABLE — possible test/report issues.'
        }
    }
}
