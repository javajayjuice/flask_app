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
                git credentialsId: '5b1570b7-f35b-43bd-b452-16f2c6f990b1', url: 'git@github.com/javajayjuice/flask_app.git', branch: 'main'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat '''
                    python -m venv venv
                    source venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                bat '''
                    source venv/bin/activate
                    pytest tests/
                '''
            }
        }
        stage('SonarQube Analysis') {
            steps {
                bat '''
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
