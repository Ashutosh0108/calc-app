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
                    sh 'python -m venv ${VENV_DIR}'
                    // Activate venv and upgrade pip
                    sh '''
                    source ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt || true
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Activate venv and run tests
                    sh '''
                    source ${VENV_DIR}/bin/activate
                    python -m unittest discover -s tests
                    '''
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
            sh "rm -rf ${VENV_DIR}"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs!'
        }
    }
}
