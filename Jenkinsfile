pipeline {
    agent {label 'agent-server1'}
    
// Gunakan tools jika target host pipeline belum ada nodejs nya
// tools {nodejs "nodejs 18.16.0"} 

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'dev', url: 'https://github.com/zcoff/simple-apps.git'
            }
        }
        stage('Build') {
            steps {
                sh '''cd apps
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''cd apps
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''cd apps
                sonar-scanner \
                -Dsonar.projectKey=Simple-Apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.14.106:9000 \
                -Dsonar.login=sqp_875aa6c9e9487857c9f39fa303c66a48d0e6b5ac'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
    }
}