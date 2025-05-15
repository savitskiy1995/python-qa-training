pipeline {
    agent any

    stages {
        stage('Setup Python') {
            steps {
                bat 'python --version'
                bat 'python -m venv venv'
                bat 'call venv\\Scripts\\activate.bat && python -m pip install --upgrade pip'
            }
        }

        stage('Install dependencies') {
            steps {
                bat '''
                    call venv\\Scripts\\activate.bat && ^
                    pip install pytest pytest-html && ^
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                    call venv\\Scripts\\activate.bat && ^
                    python -m pytest test/ --junitxml=test-results.xml --html=report.html --self-contained-html
                '''
            }
        }

        stage('Publish Reports') {
            steps {
                junit 'test-results.xml'
                publishHTML(target: [
                    reportDir: '.',    // текущая директория
                    reportFiles: 'report.html',
                    reportName: 'Pytest HTML Report'
                ])
            }
        }
    }
}
