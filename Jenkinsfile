pipeline {
    agent any
    environment {
        REGISTRY = "your-docker-registry" // Replace this
        SERVICES = ['order', 'product', 'user', 'payment'] // Update as needed
    }
    stages {
        stage('Checkout Code') {
            steps {
                git url: 'http://github.com/sarasrija/microservices.git'
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    for (svc in env.SERVICES) {
                        sh "docker build -t $REGISTRY/$svc:latest $svc"
                    }
                }
            }
        }
        stage('Test Microservices') {
            steps {
                script {
                    for (svc in env.SERVICES) {
                        sh "cd $svc && ./run-tests.sh || exit 1"
                    }
                }
            }
        }
        stage('Push Images to Registry') {
            steps {
                script {
                    for (svc in env.SERVICES) {
                        sh "docker push $REGISTRY/$svc:latest"
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f k8s/" // assumes manifests are in k8s/
            }
        }
    }
}
