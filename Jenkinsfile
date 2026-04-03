pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh '''
                sudo apt update -y
                sudo apt install python3-pip python3-venv -y

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
