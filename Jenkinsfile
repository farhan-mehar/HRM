pipeline {
    agent any

    environment {
        PYTHON = "/usr/bin/python3"
        VENV = ".venv"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Murtaza7n/Hrm-System.git'
            }
        }

        stage('Set up Virtual Environment') {
            steps {
                sh """
                python3 -m venv ${VENV}
                source ${VENV}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Migrations') {
            steps {
                sh """
                source ${VENV}/bin/activate
                python manage.py makemigrations
                python manage.py migrate
                """
            }
        }

        stage('Run Tests') {
            steps {
                sh """
                source ${VENV}/bin/activate
                python manage.py test
                """
            }
        }

        stage('Collect Static Files') {
            steps {
                sh """
                source ${VENV}/bin/activate
                python manage.py collectstatic --noinput
                """
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying application...'
                // Example: Copy project files to server or build docker image
                // sh 'scp -r * user@server:/path/to/deploy/'
                // or docker build & push
            }
        }
    }

    post {
        success {
            echo '✅ Build & Deploy Successful!'
        }
        failure {
            echo '❌ Build Failed!'
        }
    }
}

