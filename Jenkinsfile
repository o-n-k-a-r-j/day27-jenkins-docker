
pipeline {
    agent {
        label 'docker-mac'
    }

    stages {
        stage('Check Source') {
            steps {
                sh 'pwd'
                sh 'ls'
                sh 'test -f Dockerfile'
                sh 'test -f index.html'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t day27-site:v1 .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d --name day27-site -p 127.0.0.1:8095:80 day27-site:v1'
            }
        }

        stage('Verify Website') {
            steps {
                sh 'curl -fsS --retry 10 --retry-connrefused --retry-delay 1 http://127.0.0.1:8095/'
            }
        }
    }
}
