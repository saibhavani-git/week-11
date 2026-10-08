pipeline {
    agent any

    stages {

        stage('Run Selenium Tests with pytest') {

            steps {

                echo "Running Selenium Tests using pytest"

                bat 'python --version'

                bat 'python -m pip --version'

                bat 'python -m pip install -r requirements.txt'


                echo "Starting Flask application..."

                bat 'start /B python app.py'

                bat 'ping 127.0.0.1 -n 5 > nul'


                echo "Checking Flask application..."

                bat 'curl.exe -s http://127.0.0.1:5000/'


                echo "Running Selenium tests..."

                bat 'python -m pytest -v'
            }
        }


        stage('Build Docker Image') {

            steps {

                echo "Building Docker Image"

                bat 'docker build -t registration-form:v1 .'
            }
        }


        stage('Docker Login') {

            steps {

                echo "Logging into Docker Hub"

                /*
                 Use Jenkins Credentials instead of putting
                 your Docker password/token directly here.
                */

                bat 'docker login -u saibhavani12 -p YOUR_DOCKER_TOKEN'
            }
        }


        stage('Push Docker Image to Docker Hub') {

            steps {

                echo "Pushing Docker Image to Docker Hub"

                bat 'docker tag registration-form:v1 saibhavani12/sample2026:registration-form'

                bat 'docker push saibhavani12/sample2026:registration-form'
            }
        }


        stage('Deploy to Kubernetes') {

            steps {

                echo "Deploying application to Kubernetes"

                bat 'kubectl apply -f deployment.yaml --validate=false'

                bat 'kubectl apply -f service.yaml'


                echo "Checking Kubernetes resources"

                bat 'kubectl get deployments'

                bat 'kubectl get pods'

                bat 'kubectl get services'
            }
        }
    }


    post {

        success {

            echo 'Pipeline completed successfully!'
        }

        failure {

            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
