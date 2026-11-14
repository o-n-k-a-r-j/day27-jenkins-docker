
pipeline {
    agent {
        label 'docker-mac'
    }

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage('Check Source') {
            steps {
                sh 'test -f Dockerfile'
                sh 'test -f index.html'
                echo 'Source files are ready'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    docker.build("day27-site:${env.BUILD_NUMBER}", '.')
                }
            }
        }

        stage('Test Image') {
            steps {
                script {
                    docker.image("day27-site:${env.BUILD_NUMBER}").withRun('-p 127.0.0.1:8096:80') {
                        sh 'curl -fsS --retry 10 --retry-connrefused --retry-delay 1 http://127.0.0.1:8096/ | grep -F "Day 27"'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f day27-site 2>/dev/null || true'

                script {
                    docker.image("day27-site:${env.BUILD_NUMBER}").run('--name day27-site -p 127.0.0.1:8095:80')
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'curl -fsS --retry 10 --retry-connrefused --retry-delay 1 http://127.0.0.1:8095/ | grep -F "Day 27"'
            }
        }
    }

    post {
        success {
            echo 'Docker CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the stage and console output.'
        }
    }
}
