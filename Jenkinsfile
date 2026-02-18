pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Привет, Jenkins из GitHub!'
                echo 'Триггер сработал!'
                // Здесь могут быть твои реальные шаги сборки
            }
        }
        stage('Create Artifact') {
            steps {
                script {
                    def buildInfo = "Build #${BUILD_NUMBER}\n"
                    buildInfo += "Дата: ${new Date()}\n"
                    buildInfo += "Версия: 1.0.${BUILD_NUMBER}\n"
                    buildInfo += "Статус: успешно"
                    
                    writeFile file: 'build-info.txt', text: buildInfo
                    echo "Файл build-info.txt создан"
                }
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'build-info.txt', fingerprint: true
        }
        success {
            sh '''
                curl -s -X POST https://api.telegram.org/bot<ТВОЙ_ТОКЕН>/sendMessage \
                    -d chat_id=383711942 \
                    -d text="✅ Сборка #${BUILD_NUMBER} успешно завершена%0AСсылка: ${BUILD_URL}"
            '''
        }
        failure {
            sh '''
                curl -s -X POST https://api.telegram.org/bot<ТВОЙ_ТОКЕН>/sendMessage \
                    -d chat_id=383711942 \
                    -d text="❌ Сборка #${BUILD_NUMBER} упала%0AПодробности: ${BUILD_URL}"
            '''
        }
    }
}
