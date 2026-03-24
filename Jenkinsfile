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

    post {
            success {
                discordSend(
                    webhookURL: 'ТУКА_ЗАЛЕПИ_ГО_DISCORD_WEBHOOK_LINKOT',
                    title: "Успешен Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    description: "Браво Стефан! Апликацијата е успешно тестирана и спакувана. ✅",
                    result: 'SUCCESS',
                    scms: true
                )
            }
            failure {
                discordSend(
                    webhookURL: 'ТУКА_ЗАЛЕПИ_ГО_DISCORD_WEBHOOK_LINKOT',
                    title: "Build ФЕЈЛНА: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    description: "Нешто се расипа! Провери ги логовите во Jenkins. ❌",
                    result: 'FAILURE',
                    scms: true
                )
            }
        }
}