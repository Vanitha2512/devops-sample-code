pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Creating virtual environment and installing dependencies..."
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip --break-system-packages
                    pip install -r requirements.txt --break-system-packages
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh '''
                    . venv/bin/activate
                    python3 -m unittest discover -s .
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                sh '''
                    mkdir -p ${WORKSPACE}/python-app-deploy
                    cp ${WORKSPACE}/app.py ${WORKSPACE}/python-app-deploy/
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo "Running application..."
                sh '''
                    . venv/bin/activate
                    nohup python3 ${WORKSPACE}/python-app-deploy/app.py > ${WORKSPACE}/python-app-deploy/app.log 2>&1 &
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo "Testing application..."
                sh '''
                    . venv/bin/activate
                    python3 test_app.py
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}

