pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
            }
        }
        stage('Compile') {
            steps {
                sh './mvnw clean compile'
            }
        }
        stage('Unit Tests') {
            steps {
                sh './mvnw test'
            }
        }
        stage('Package') {
            steps {
                sh './mvnw package -DskipTests'
                echo 'App is ready! Found in target/ folder.'
            }
        }
    }

    // СИТЕ пост-акции одат во ЕДЕН блок
    post {
        always {
            echo 'Pipeline finished!'
        }
        success {
            echo 'Bravo Stefan! Tests passed.'
            // Discord нотификација за успех
            discordSend(
                webhookURL: ,
                title: "Успешен Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                description: "Браво Стефан! Апликацијата е успешно тестирана и спакувана. ✅",
                result: 'SUCCESS',
                scms: true
            )
        }
        failure {
            echo 'Error! Check the console output.'
            // Discord нотификација за неуспех
            discordSend(
                webhookURL: ,
                title: "Build ФЕЈЛНА: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                description: "Нешто се расипа! Провери ги логовите во Jenkins. ❌",
                result: 'FAILURE',
                scms: true
            )
        }
    }
}