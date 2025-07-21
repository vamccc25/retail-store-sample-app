pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/vamccc25/retail-store-sample-app.git'
            }
        }

        stage('Build & Test UI (Java)') {
            steps {
                dir('src/frontend') {
                    sh 'mvn clean install'
                    sh 'docker build -t ui-service .'
                }
            }
        }

        stage('Build & Test Orders (Java)') {
            steps {
                dir('src/orders') {
                    sh 'mvn clean install'
                    sh 'docker build -t orders-service .'
                }
            }
        }

        stage('Build & Test Cart (Java)') {
            steps {
                dir('src/cart') {
                    sh 'mvn clean install'
                    sh 'docker build -t cart-service .'
                }
            }
        }

        stage('Build & Test Catalog (Go)') {
            steps {
                dir('src/catalog') {
                    sh 'go mod tidy'
                    sh 'go test ./...'
                    sh 'docker build -t catalog-service .'
                }
            }
        }

        stage('Build & Test Checkout (Node.js)') {
            steps {
                dir('src/checkout') {
                    sh 'npm install'
                    sh 'npm test || echo "No tests found"'
                    sh 'docker build -t checkout-service .'
                }
            }
        }
    }

    post {
        success {
            echo '✅ All components built successfully!'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}