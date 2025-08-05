pipeline {
    agent {
        docker {
            image 'node:16-alpine'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Build') {
            steps {
                withEnv(['npm_config_cache=./.npm']) {
                    sh 'npm install'
                }
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    timeout(time: 15, unit: 'MINUTES') {
                        input message: 'Lanjutkan ke tahap Deploy?'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    try {
                        echo 'Aplikasi akan berjalan selama 1 menit...'
                        sh "nohup npm start &"
                        // Menunggu 60 detik
                        sleep(60)
                    } finally {
                        echo 'Menghentikan aplikasi...'
                        sh "killall node"
                    }
                }
            }
        }
    }
}
