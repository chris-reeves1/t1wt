pipeline {
    agent any
    environment {
        // Define environment variable for SonarQube host
        SONARQUBE_HOST = 'http://35.177.225.173:9000/' // Replace <Your-Host> with the actual host name or IP address
    }
    stages {
        stage('Build Image') {
            steps {
                script {
                    // Remove any existing containers to avoid conflicts, then build the Docker image
                    sh 'docker rm -f $(docker ps -aq) || true'
                    sh 'docker build -t myapp .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Run the Docker container in detached mode
                    sh 'docker run -d --name myapp -p 80:5500 myapp'
                }
            }
        }

        stage('Wait for App') {
            steps {
                script {
                    // Wait a few seconds to ensure the Flask app is fully up and running
                    sh 'sleep 10'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Retrieve SonarQube token securely from Jenkins credentials
                    withCredentials([string(credentialsId: 'your-credentials-id', variable: 'SONAR_TOKEN')]) {
                        // Run SonarScanner using Docker, analyzing the Python application
                        sh """
                        docker run --rm \
                          -e SONAR_HOST_URL=${SONARQUBE_HOST} \
                          -e SONAR_LOGIN=${SONAR_TOKEN} \
                          -v ${WORKSPACE}:/usr/src \
                          sonarsource/sonar-scanner-cli \
                          -Dsonar.projectKey=myapp \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=${SONARQUBE_HOST} \
                          -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Execute Tests') {
            steps {
                script {
                    // Assuming you have a requirements.txt for test dependencies
                    sh 'pip install -r requirements.txt'
                    // Run your Python application tests
                    sh 'python3 -m unittest discover -s tests'
                }
            }
        }
    }
    post {
        always {
            // Clean up Docker container
            sh 'docker rm -f myapp || true'
        }
    }
}


        stage('Wait for App') {
            steps {
                script {
                    // Wait a few seconds to ensure the Flask app is fully up and running
                    sh 'sleep 10'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarScanner using Docker, analyzing the Python application
                    sh "docker run --rm \
                      -e SONAR_HOST_URL=${SONARQUBE_HOST} \
                      -e SONAR_LOGIN=${SONAR_TOKEN} \
                      -v ${WORKSPACE}:/usr/src \
                      sonarsource/sonar-scanner-cli \
                      -Dsonar.projectKey=myapp \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=${SONARQUBE_HOST} \
                      -Dsonar.login=${SONAR_TOKEN} \
                      -Dsonar.python.coverage.reportPaths=coverage.xml \
                      -Dsonar.language=py"
                }
            }
        }

        stage('Execute Tests') {
            steps {
                script {
                    // Assuming you have a requirements.txt for test dependencies
                    sh 'pip install -r requirements.txt'
                    // Run your Python application tests
                    sh 'python3 -m unittest discover -s tests'
                }
            }
        }
    }
}
}
