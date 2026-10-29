```groovy
stage('Build Docker Image') {
    steps {
        echo "Building Docker Image"
        bat 'docker build -t seleniumdemoapp1:v1 .'
    }
}

stage('Docker Login') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKERHUB_USER',
            passwordVariable: 'DOCKERHUB_TOKEN'
        )]) {
            bat 'echo %DOCKERHUB_TOKEN% | docker login -u %DOCKERHUB_USER% --password-stdin'
        }
    }
}

stage('Push Docker Image to Docker Hub') {
    steps {
        echo "Pushing Docker Image to Docker Hub"

        bat 'docker tag seleniumdemoapp1:v1 peddianvitha/seleniumdemoapp1:v1'

        bat 'docker push peddianvitha/seleniumdemoapp1:v1'
    }
}
```