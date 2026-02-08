pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Build') {
            steps {
                bat './mvnw clean install'
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }

        stage('Documentation') {
            steps {
                script {
                    bat './mvnw javadoc:javadoc'
                    bat 'if exist doc del /Q doc'
                    bat 'mkdir doc'
                    bat 'xcopy /E /I /Y target\\site doc'
                    bat 'powershell -Command "Compress-Archive -Path doc\\* -DestinationPath doc.zip"'
                    archiveArtifacts artifacts: 'doc.zip', fingerprint: true
                }
            }
        }

        stage('Html Publisher') {
            steps {
                script {
                    publishHTML (target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportFiles: 'index.html',
                        reportName: 'Documentation Report',
                        reportTitles: 'Documentation Report'
                    ])
                }
            }
        }
    }
}