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
                input message: 'Apakah Anda setuju untuk melanjutkan ke tahap Deploy? (Klik "Proceed" untuk mengizinkan)'
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                sh './jenkins/scripts/kill.sh'
                // Tambahkan perintah sleep di bawah ini
                sleep time: 60, unit: 'SECONDS'
            }
        }
    }
}
