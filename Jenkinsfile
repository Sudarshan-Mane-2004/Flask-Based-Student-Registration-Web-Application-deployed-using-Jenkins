pipeline {
    agent any

    environment {
        DB_NAME = 'studentsdb'
        DB_USER = 'root'
        DB_PASS = 'password'
    }

    stages {

        stage('Install System Dependencies') {
            steps {
                sh '''
                sudo DEBIAN_FRONTEND=noninteractive apt update -y
                sudo DEBIAN_FRONTEND=noninteractive apt install python3-pip python3-venv mysql-server -y
                '''
            }
        }

        stage('Start MySQL') {
            steps {
                sh '''
                sudo systemctl start mysql
                '''
            }
        }

        stage('Configure MySQL User') {
            steps {
                sh '''
                sudo mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';"
                sudo mysql -e "FLUSH PRIVILEGES;"
                '''
            }
        }

        stage('Create Database & Table') {
            steps {
                sh '''
                sudo mysql -u root -ppassword -e "
                CREATE DATABASE IF NOT EXISTS studentsdb;
                USE studentsdb;
                CREATE TABLE IF NOT EXISTS students (
                    id INT AUTO_INCREMENT PRIMARY KEY,
                    name VARCHAR(100),
                    email VARCHAR(100),
                    phone VARCHAR(15),
                    course VARCHAR(50),
                    address TEXT,
                    contact VARCHAR(50)
                );
                "
                '''
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                . venv/bin/activate
                pkill -f app.py || true
                nohup python app.py > app.log 2>&1 &
                '''
            }
        }

    }
}