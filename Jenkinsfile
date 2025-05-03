pipeline {
    agent any

    environment {
        PYTHON_ENV = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository and specify the branch
                git branch: 'main', url: 'https://github.com/AkashChand6n/Py-application.git'
            }
        }

        stage('Setup') {
            steps {
                script {
                    // Create a virtual environment and install dependencies
                    sh 'python3 -m venv $PYTHON_ENV'
                    sh '. $PYTHON_ENV/bin/activate && pip install --upgrade pip'
                    sh '. $PYTHON_ENV/bin/activate && pip install -r requirements.txt'
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    // Run flake8 for linting
                    sh '. $PYTHON_ENV/bin/activate && flake8 --exclude=venv .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run pytest for testing
                    sh '. $PYTHON_ENV/bin/activate && pytest --maxfail=1 --disable-warnings -q'
                }
            }
        }

        stage('Publish Coverage') {
            steps {
                script {
                    // Run coverage report generation
                    sh '. $PYTHON_ENV/bin/activate && coverage run -m pytest'
                    sh '. $PYTHON_ENV/bin/activate && coverage report'
                    sh '. $PYTHON_ENV/bin/activate && coverage html'  // To generate the HTML report (optional)
                }
            }
        }
    }

    post {
        always {
            // Clean up the workspace after the job is complete
            cleanWs()
        }
    }
}
