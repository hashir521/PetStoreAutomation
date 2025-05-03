pipeline {
    agent any

    tools {
        maven 'mymaven'     // Must match the name you gave in Jenkins Global Tool Config
        jdk 'jdk-21'        // Must match the configured JDK name in Jenkins
    }

    environment {
        // You can add custom environment variables here if needed
       // ALLURE_RESULTS = 'target/allure-results'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Clones your Git repository
            }
        }

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

                stage('Allure Report') {
                    steps {
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
    }
}
