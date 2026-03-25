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
            // Оваа линија му кажува на Jenkins: „Оди во Credentials, најди го тој со ID 'discord-webhook-id' и стави го во променливата DISCORD_URL“
            withCredentials([string(credentialsId: 'discord-webhook-id', variable: 'https://discord.com/api/webhooks/1486155351916019732/UzufAjZjAirWKS7MyYVNbhmazrnzeb1fiV9pInWjUpWFR4RLveqyrdTa8oEOVleU43HV')]) {
                discordSend(
                    webhookURL: "${https://discord.com/api/webhooks/1486155351916019732/UzufAjZjAirWKS7MyYVNbhmazrnzeb1fiV9pInWjUpWFR4RLveqyrdTa8oEOVleU43HV}",
                    title: "Успешен Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    description: "Браво Стефан! Сега работиме безбедно и професионално. ✅",
                    result: 'SUCCESS',
                    scms: true
                )
            }
        }
        failure {
            withCredentials([string(credentialsId: 'discord-webhook-id', variable: 'https://discord.com/api/webhooks/1486155351916019732/UzufAjZjAirWKS7MyYVNbhmazrnzeb1fiV9pInWjUpWFR4RLveqyrdTa8oEOVleU43HV')]) {
                discordSend(
                    webhookURL: "${https://discord.com/api/webhooks/1486155351916019732/UzufAjZjAirWKS7MyYVNbhmazrnzeb1fiV9pInWjUpWFR4RLveqyrdTa8oEOVleU43HV}",
                    title: "Build ФЕЈЛНА: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    description: "Нешто се расипа! Провери ги логовите во Jenkins. ❌",
                    result: 'FAILURE',
                    scms: true
                )
            }
        }    }
}