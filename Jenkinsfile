
pipeline {
    agent any

    stages {
        stage('Run Selenium Tests with pytest') {
            steps {
                echo 'Running Selenium Tests using pytest'

                bat '"C:\\Users\\peddi\\AppData\\Local\\Python\\bin\\python.exe" -m pip install -r requirements.txt'

                bat 'start /B "" "C:\\Users\\peddi\\AppData\\Local\\Python\\bin\\python.exe" app.py'

                bat 'ping 127.0.0.1 -n 6 > nul'

                bat '"C:\\Users\\peddi\\AppData\\Local\\Python\\bin\\python.exe" -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                bat 'docker build -t seleniumdemoapp1:v1 .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_TOKEN'
                    )
                ]) {
                    bat 'echo %DOCKERHUB_TOKEN% | docker login -u %DOCKERHUB_USER% --password-stdin'
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                bat 'docker tag seleniumdemoapp1:v1 peddianvitha/seleniumdemoapp1:v1'

                bat 'docker push peddianvitha/seleniumdemoapp1:v1'
            }
        }

       stage('Deploy to Kubernetes') {
    steps {
        echo 'Deploying application to Kubernetes'
        bat 'set "KUBECONFIG=C:\\Users\\peddi\\.kube\\config" && kubectl apply -f deployment.yaml'
        bat 'set "KUBECONFIG=C:\\Users\\peddi\\.kube\\config" && kubectl get deployments'
    }
}
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}
