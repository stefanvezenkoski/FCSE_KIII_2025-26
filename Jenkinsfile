pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Jenkins автоматски го презема кодот бидејќи го поврзавме во Чекор 3
                echo 'Checking out code from GitHub...'
            }
        }
        stage('Compile') {
            steps {
                // Ова ја компајлира Java апликацијата
                sh './mvnw clean compile'
            }
        }
        stage('Unit Tests') {
            steps {
                // Ова ги извршува тестовите што ги напишавме
                sh './mvnw test'
            }
        }
        stage('Package') {
            steps {
                // Ова прави .jar фајл (финалниот продукт)
                sh './mvnw package -DskipTests'
                echo 'App is ready! Found in target/ folder.'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
        success {
            echo 'Bravo Stefan! Tests passed.'
        }
        failure {
            echo 'Error! Check the console output.'
        }
    }
}