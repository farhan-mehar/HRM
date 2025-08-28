pipeline {
    agent any

    environment {
        PYTHON = "/usr/bin/python3"
        VENV = ".venv"

        // MySQL Database Credentials (secure way)
        DB_NAME = "hrm_db"
        DB_USER = "hrm_user"
        DB_PASSWORD = "your_password_here"
        DB_HOST = "localhost"
        DB_PORT = "3306"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/farhan-mehar/HRM.git'
            }
        }

        stage('Set up Virtual Environment') {
            steps {
                sh """
                ${PYTHON} -m venv ${VENV}
                . ${VENV}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                pip install mysqlclient PyMySQL
                """
            }
        }

        stage('Run Migrations') {
            steps {
                sh """
                . ${VENV}/bin/activate
                python manage.py makemigrations
                python manage.py migrate
                """
            }
        }

        stage('Run Tests') {
            steps {
                sh """
                . ${VENV}/bin/activate
                python manage.py test
                """
            }
        }

        stage('Collect Static Files') {
            steps {
                sh """
                . ${VENV}/bin/activate
                python manage.py collectstatic --noinput
                """
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                echo '🚀 Deploying application...'
                // Example deployment steps:
                // sh 'scp -r * user@server:/path/to/deploy/'
                // OR docker build & push
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
