pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')  // Adding this in Jenkins Credentials
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the Docker containers...'
                sh 'docker-compose build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests with Pytest...'
                sh 'docker-compose run backend pytest'
            }
        }

        stage('Code Quality') {
            steps {
                echo 'Running code quality analysis with SonarQube...'
                sh '''
                sonar-scanner \
                  -Dsonar.projectKey=NutriHealth \
                  -Dsonar.sources=./backend \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }

        stage('Security') {
            steps {
                echo 'Running security scan with Snyk...'
                sh 'snyk test --file=backend/requirements.txt || true'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to test environment...'
                sh 'docker-compose up -d'
            }
        }

        stage('Release') {
            steps {
                echo 'Simulating release step...'
                sh 'echo "Release to production done!"'
            }
        }

        stage('Monitoring') {
            steps {
                echo 'Checking if app is live...'
                sh 'curl http://localhost:8000/health || true'
            }
        }
    }
}
