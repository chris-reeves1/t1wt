pipeline {
    agent any
    environment {
        // Define environment variable for SonarQube host
        SONARQUBE_HOST = 'http://13.40.19.217:9000/' // SonarQube server's IP and port
    }
    stages {
        stage('Build Image') {
            steps {
                script {
                    // Remove any existing containers and build the Docker image
                    //sh 'docker rm -f $(docker ps -aq) || true'
                    sh 'docker build -t myapp .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Run the Docker container in detached mode
                    sh 'docker run -d --name myapp -p 80:5500 --network test myapp'
                }
            }
        }

        stage('Wait for App') {
            steps {
                script {
                    // Ensure the app is fully up and running
                    sh 'sleep 10'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Retrieve SonarQube token securely and run SonarScanner
                    withCredentials([string(credentialsId: '72652812-cd9a-441c-9c20-ca21c0facdbe', variable: 'SONAR_TOKEN')]) {
                        sh """
                        docker run --rm --network test \
                          -e SONAR_HOST_URL=\${SONARQUBE_HOST} \
                          -e SONAR_LOGIN=\${SONAR_TOKEN} \
                          -v \${WORKSPACE}:/usr/src \
                          sonarsource/sonar-scanner-cli \
                          -Dsonar.projectKey=myapp \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=\${SONARQUBE_HOST} \
                          -Dsonar.login=\${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        /* Uncomment and modify this stage as needed for your test execution requirements
        stage('Execute Tests') {
            steps {
                script {
                    sh 'pip install -r requirements.txt'
                    sh 'python3 -m unittest discover -s tests'
                }
            }
        }
        */
    }
    post {
        always {
            // Clean up the Docker container after the pipeline execution
            sh 'docker rm -f myapp || true'
        }
    }
}
