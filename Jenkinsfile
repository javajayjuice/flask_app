pipeline {
    agent any
    tools {
        python 'Python 3.11' // Make sure it's installed under Global Tools
    }
    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_AUTH_TOKEN = credentials('sonarqube')
    }
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'git-ssh-key', url: 'git@github.com:yourusername/flask_app.git', branch: 'main'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                    python -m venv venv
                    source venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                    source venv/bin/activate
                    pytest tests/
                '''
            }
        }
        stage('SonarQube Analysis') {
            steps {
                sh '''
                    source venv/bin/activate
                    pylint app.py > pylint-report.txt || true
                    sonar-scanner \
                        -Dsonar.login=$SONAR_AUTH_TOKEN \
                        -Dsonar.host.url=$SONAR_HOST_URL
                '''
            }
        }
    }
}
