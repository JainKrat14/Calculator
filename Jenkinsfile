pipeline {
    agent any

    environment {
        PYTHON_ENV = 'python3'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git 'https://github.com/your-username/my-python-project.git'
            }
        }
        
        stage('Set Up Python Environment') {
            steps {
                // Install Python and dependencies
                script {
                    def pythonCheck = sh(script: "command -v python3", returnStatus: true)
                    if (pythonCheck != 0) {
                        sh '''
                        apt update
                        apt install -y python3
                        '''
                    }
                    sh 'python3 --version'
                }
                sh 'python3 -m venv venv'  // Create a virtual environment
                sh './venv/bin/pip install -r requirements.txt'  // Install dependencies
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests using pytest
                sh './venv/bin/pytest test_app.py'
            }
        }

        stage('Clean Up') {
            steps {
                // Clean up virtual environment after the build
                sh 'rm -rf venv'
            }
        }
    }

    post {
        success {
            echo 'Tests passed successfully!'
        }
        failure {
            echo 'Tests failed. Check the console output for details.'
        }
    }
}
