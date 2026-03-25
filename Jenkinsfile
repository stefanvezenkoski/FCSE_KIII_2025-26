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
                sh 'chmod +x mvnw'
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

    post {
        always {
            echo 'Pipeline finished!'
        }
        success {
            withCredentials([string(credentialsId: 'my-discord-webhook', variable: 'DISCORD_URL')]) {
                discordSend(
                    webhookURL: "${DISCORD_URL}",
                    title: "Успешен Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    description: "Браво Стефан! Сега кодот е конечно безбеден и чист. ✅ Build-от го направи: ${env.USER_ID}. Погледни ги промените тука: ${env.BUILD_URL",
                    result: 'SUCCESS',
                )
            }
        }
        failure {
            withCredentials([string(credentialsId: 'my-discord-webhook', variable: 'DISCORD_URL')]) {
                discordSend(
                    webhookURL: "${DISCORD_URL}",
                    title: "Build ФЕЈЛНА: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    description: "Провери ги логовите во Jenkins. ❌. Build-от го направи: ${env.USER_ID}. Погледни ги промените тука: ${env.BUILD_URL",
                    result: 'FAILURE',
                )
            }
        }
    }
}