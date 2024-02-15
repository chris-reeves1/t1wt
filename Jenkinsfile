pipeline {
    agent {
        label "worker" }
    stages {
        stage('Build Image') {
            steps {
                script {
                    sh 'docker rm -f $(docker ps -aq) || true'
                    sh "docker build -t myapp ."
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker run -d --name myapp -p 80:5500 myapp"
                }
            }
        }

        stage('Wait for App') {
            steps {
                script {
                    sh "sleep 10"
                }
            }
        }

        stage('Execute Tests') {
            steps {
                script {
                    sh "sudo apt install python3-pip -y"
                    sh "pip install -r requirements-test.txt"
                    sh "python3 -m unittest test_app.py"
}
}
        }
    }
}
