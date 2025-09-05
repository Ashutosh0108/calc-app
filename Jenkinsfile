pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull code from your repository
                checkout scm
            }
        }

        stage('Setup Python Virtual Environment') {
            steps {
                script {
                    // Create virtual environment
                    bat "python -m venv %VENV_DIR%"
                    // Activate venv and install dependencies
                    bat """
                    call %VENV_DIR%\\Scripts\\activate.bat
                    python -m pip install --upgrade pip
                    python -m pip install -r requirements.txt || exit 0
                    """
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Activate venv and run tests
                    bat """
                    call %VENV_DIR%\\Scripts\\activate.bat
                    python -m unittest discover -s tests
                    """
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Build step completed. You can add packaging here if needed.'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            bat "rmdir /s /q %VENV_DIR%"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs!'
        }
    }
}
