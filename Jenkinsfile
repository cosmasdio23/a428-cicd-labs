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
    }
}
