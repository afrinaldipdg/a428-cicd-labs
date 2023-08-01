pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input message: 'Apakah Anda setuju untuk melanjutkan ke tahap Deploy? (Klik "Proceed" untuk mengizinkan)', parameters: [
                        [$class: 'BooleanParameterDefinition', name: 'APPROVAL', defaultValue: false, description: 'Klik "Proceed" untuk mengizinkan']
                    ]
                    if (!userInput.APPROVAL) {
                        error("Pengguna telah membatalkan proses. Tahap Deploy dibatalkan.")
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                sh './jenkins/scripts/kill.sh'
                sleep time: 60, unit: 'SECONDS'
            }
        }
    }
}
