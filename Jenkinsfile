pipeline {
    agent any

    tools {
        maven 'mymaven'     // Must match the name you gave in Jenkins Global Tool Config
        jdk 'myjava'        // Must match the configured JDK name in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Clones your Git repository
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
                        junit 'target/surefire-reports/*.xml'  // Parses test results
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
