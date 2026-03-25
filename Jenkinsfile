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
                       // Ја повикуваме функцијата одоздола
                       def authorName = getCommitAuthor()

                       withCredentials([string(credentialsId: 'my-discord-webhook', variable: 'DISCORD_URL')]) {
                           discordSend(
                               webhookURL: "${DISCORD_URL}",
                               title: "Билд #${env.BUILD_NUMBER} - УСПЕШЕН! ✅",
                               description: """
       **Проект:** ${env.JOB_NAME}
       **Девелопер:** ${authorName}
       **Линкот е поправен:** ${env.BUILD_URL}
       __________________________________
       Браво Стефан! Пајплајнот конечно работи без грешка.
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

@NonCPS
def getCommitAuthor() {
    def changeLogSets = currentBuild.changeSets
    if (changeLogSets != null && !changeLogSets.isEmpty()) {
        def entries = changeLogSets[0].items
        if (entries.length > 0) {
            return entries[0].author.fullName
        }
    }
    return "Unknown Developer"
}