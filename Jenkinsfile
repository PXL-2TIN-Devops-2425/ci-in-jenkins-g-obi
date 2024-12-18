pipeline {
    agent any
    tools {
        nodejs 'TINnode-devops'
    }
    stages {
        stage('opdracht 5') {
            steps {
                echo "good luck..."
            }
        }
        stage('fetching source') {
            steps {
                git branch: 'main', 
                    url: 'git@github.com:alessiovep/calculator-app-finished.git',
                    credentialsId: 'github'
            }
        }
        stage('verify nodejs') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }
        stage('install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('unittest') {
            steps {
                sh 'npm test'
                junit 'test-results.xml'
            }
        }
        stage('create bundle') {
            steps {
                sh 'mkdir -p bundle'
                sh 'cp -r app.js server.js routes.js public package.json package-lock.json bundle/'
                sh 'zip -r bundle.zip bundle'
            }
        }
    }
    post {
        failure {
            script {
                def timeStamp = new Date().format("yyyy-MM-dd HH:mm:ss")
                sh "echo 'pipeline poging is mislukt op ${timeStamp}' >> ~/jenkinserrorlog"
            }
        }
        success {
            archiveArtifacts artifacts: 'bundle.zip', fingerprint: true
        }
    }
}
