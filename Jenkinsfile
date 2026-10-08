pipeline {
    agent any

    stages {

       stage('Run Selenium Tests with pytest') {
    steps {

        echo "Installing dependencies..."
        bat 'python -m pip install -r requirements.txt'

        echo "Checking project files..."
        bat 'dir'
        bat 'dir templates'
        bat 'dir test'

        echo "Starting Flask application..."
        bat 'start /B "" python app.py > flask.log 2>&1'

        echo "Waiting for Flask..."
        bat 'ping 127.0.0.1 -n 6 > nul'

        echo "===== FLASK LOG ====="
        bat 'type flask.log'

        echo "===== CHECKING PORT 5000 ====="
        bat 'netstat -ano | findstr :5000'

        echo "===== CHECKING FLASK ====="
        bat 'curl.exe -i http://127.0.0.1:5000/'

        echo "===== RUNNING SELENIUM TESTS ====="
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

        withCredentials([
            usernamePassword(
                credentialsId: 'dockerhub-creds',
                usernameVariable: 'DOCKER_USERNAME',
                passwordVariable: 'DOCKER_TOKEN'
            )
        ]) {
            bat 'docker login -u "%DOCKER_USERNAME%" -p "%DOCKER_TOKEN%"'
        }
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
