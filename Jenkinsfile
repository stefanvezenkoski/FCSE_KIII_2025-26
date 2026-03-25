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
                   script {
                       // Оваа магија го извлекува името на тој што направил push
                       def changeLogSets = currentBuild.changeSets
                       def authorName = "Unknown"
                       if (!changeLogSets.isEmpty()) {
                           authorName = changeLogSets[0].items[0].author.fullName
                       }

                       withCredentials([string(credentialsId: 'my-discord-webhook', variable: 'DISCORD_URL')]) {
                           discordSend(
                               webhookURL: "${DISCORD_URL}",
                               title: "Build #${env.BUILD_NUMBER} - УСПЕШЕН! ✅",
                               description: """
       **Проект:** ${env.JOB_NAME}
       **Девелопер:** ${authorName}
       **Линк до билдот:** ${env.BUILD_URL}
       __________________________________
       Браво Стефан! Кодот е сега безбеден и чист.
                               """,
                               result: 'SUCCESS'
                           )
                       }
                   }
               }
        failure {
            withCredentials([string(credentialsId: 'my-discord-webhook', variable: 'DISCORD_URL')]) {
                discordSend(
                    webhookURL: "${DISCORD_URL}",
                    title: "Build ФЕЈЛНА: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    description: "Провери ги логовите во Jenkins. ❌. Build-от го направи: ${env.USER_ID}. Погледни ги промените тука: ${env.RUN_DISPLAY_URL}",
                    result: 'FAILURE',
                )
            }
        }
    }
}