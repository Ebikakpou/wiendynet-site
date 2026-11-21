
pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                echo 'Building the latest commit from main'
                sh 'git log -1 --oneline'
            }
        }

        stage('Validate') {
            steps {
                sh 'test -f index.html'
                sh 'grep -q "<title>" index.html'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    rsync -av --delete \
                    --exclude=.git \
                    --exclude=Jenkinsfile \
                    --exclude=README.md \
                    ./ /var/www/html/
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'curl -sf http://localhost/ | grep -o "<title>.*</title>"'
            }
        }
    }

    post {
        success {
            echo 'Site updated successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
