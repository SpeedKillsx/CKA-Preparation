pipeline{
    agent any
    stages{
        stage("checkout"){
            steps{
                script{
                    echo "Checking out the code from SCM"
                    checkout scm
                }
            }
        }
        stage("Install Dependencies") {
            steps {
                script {
                    echo "Installing dependencies for the project..."
                    sh 'pwd'
                    sh 'ls'
                    sh 'python3 -m venv myenv'
                    sh '. myenv/bin/activate && pip install -r requirements.txt'
                    echo "Dependencies installed successfully"
                }
            }
        }
        stage("Build Docker Image"){
            steps {
                script {
                    echo "Building Docker image for the project..."
                    sh 'docker build -t speedskillsx/streamlit-application:latest .'
                    echo "Docker image built successfully"
                }
            }
        }

        stage("Push Docker Image"){
            steps {
                script {
                    echo "Pushing Docker image to Docker Hub..."
                    sh 'docker login -u speedskillsx -p $DOCKERHUB_PASSWORD'
                    sh 'docker push speedskillsx/streamlit-application:latest'
                    echo "Docker image pushed successfully"
                }
            }
        }   
    }
}