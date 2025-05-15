pipeline {
    agent any

    environment {
        VENV = "venv"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/savitskiy1995/python-qa-training.git'
            }
        }

        stage('Setup Python') {
            steps {
                sh 'python3 -m venv $VENV'
                sh './$VENV/bin/pip install --upgrade pip'
                // Если есть requirements.txt — ставим зависимости, если нет — эта команда ничего не сломает
                sh './$VENV/bin/pip install -r requirements.txt || true'
                // Обязательно ставим pytest и плагин для html отчёта
                sh './$VENV/bin/pip install pytest pytest-html'
            }
        }

        stage('Run Tests') {
            steps {
                // Запускаем тесты и создаём html отчёт
                sh './$VENV/bin/pytest test --html=report.html --self-contained-html'
            }
        }

        stage('Archive Report') {
            steps {
                archiveArtifacts artifacts: 'report.html', allowEmptyArchive: false
            }
        }
    }
}
