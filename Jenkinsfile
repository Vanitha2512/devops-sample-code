pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                sh '''
                    rm -rf venv || true
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    pip show flask
                    pip show werkzeug
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests inside virtual environment...'
                sh '''
                    . venv/bin/activate
                    python3 -m unittest discover -s .
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                    mkdir -p python-app-deploy
                    cp app.py python-app-deploy/
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo 'Running application...'
                sh '''
                    nohup python3 python-app-deploy/app.py > python-app-deploy/app.log 2>&1 &
                    echo $! > python-app-deploy/app.pid
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Testing deployed application...'
                sh '''
                    . venv/bin/activate
                    python3 test_app.py
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
