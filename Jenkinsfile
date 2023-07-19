node {
    docker.image('node:16-buster-slim').withRun('-p 3000:3000') {
        stage('Build') {
            docker.image('node:16-buster-slim').inside("-p 3000:3000") {
                sh 'npm install'
            }
        }

        stage('Test') {
            docker.image('node:16-buster-slim').inside("-p 3000:3000") {
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('Manual Approval') {
            input message: 'Apakah Anda setuju untuk melanjutkan ke tahap Deploy? (Klik "Proceed" untuk mengizinkan)'
        }

        stage('Deploy') {
            docker.image('node:16-buster-slim').inside("-p 3000:3000") {
                sh './jenkins/scripts/deliver.sh'
                sh './jenkins/scripts/kill.sh'
                // Tambahkan perintah sleep di bawah ini
                sleep(time: 60, unit: 'SECONDS')
            }
        }
    }
}
