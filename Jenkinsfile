pipeline {
    agent any

    environment {
        APP_PORT = '5000'
    

        stage('Install Dependencies') {
            steps {
                sh '''
                sudo apt update
                sudo apt install python3-pip -y
                pip3 install -r requirements.txt
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                nohup python3 app.py > app.log 2>&1 &
                '''
            }
        }
    }
}