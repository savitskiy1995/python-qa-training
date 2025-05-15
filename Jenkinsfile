pipeline {
    agent any

    stages {
        stage('Setup Python') {
            steps {
                bat 'python --version'
                bat 'python -m venv venv'
                bat 'call venv\\Scripts\\activate && python -m pip install --upgrade pip'
            }
        }

        stage('Install dependencies') {
            steps {
                bat '''
                call venv\\Scripts\\activate && 
                pip install pytest pytest-html junitxml && 
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                call venv\\Scripts\\activate && 
                python -m pytest test/ --junitxml=test-results.xml --html=report.html
                '''
            }
        }

        stage('Publish Reports') {
            steps {
                junit 'test-results.xml'
                publishHTML(target: [
                    reportDir: '',
                    reportFiles: 'report.html',
                    reportName: 'Pytest HTML Report'
                ])
            }
        }
    }
}
