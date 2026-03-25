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
                               title: "Build #${env.BUILD_NUMBER} - SUCCESSFUL! ✅",
                               description: """
       **Project name:** ${env.JOB_NAME}
       **Developer:** ${authorName}
       **Link:** ${currentBuild.absoluteUrl}
     __________________________________
       Congratulations! The pipeline is finally working without errors.
                               """,
                               result: 'SUCCESS'
                           )
                       }
                   }
               }
        failure {
        script {
           def authorName = getCommitAuthor()

            withCredentials([string(credentialsId: 'my-discord-webhook', variable: 'DISCORD_URL')]) {
                discordSend(
                    webhookURL: "${DISCORD_URL}",
                    title: "#${env.BUILD_NUMBER} - Build failed:" ❌,
                               description: """
       **Project name:** ${env.JOB_NAME}
       **Developer:** ${authorName}
       **Check the changes here:** ${currentBuild.absoluteUrl}
     __________________________________

                               """,
                    result: 'FAILURE',
                )
            }
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