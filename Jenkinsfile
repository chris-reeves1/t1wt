pipeline {
    agent any
    stages {
        stage('Build Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in the current directory
                    sh 'docker rm -f $(docker ps -aq) || true'
                    sh "docker build -t myapp ."
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Run the Docker container in detached mode
                    sh "docker run -d --name myapp -p 80:5500 myapp"
                }
            }
        }

        stage('Wait for App') {
            steps {
                script {
                    // Wait a few seconds to ensure the Flask app is fully up and running
                    sh "sleep 10"
                }
            }
        }

        stage('Execute Tests') {
            steps {
                script {
                    // Install test dependencies
                    sh "sudo apt install python3-pip -y"
                    sh "pip install -r requirements-test.txt"
                    // Run tests, assuming tests are in a directory named 'tests'
                    sh "python3 -m unittest discover -s tests"
}
}
        }
    }
}
