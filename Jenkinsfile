pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
    }

    tools {
        nodejs 'NodeJS 18'  // Name must match what you configure in Jenkins > Global Tool Configuration
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/vamccc25/retail-store-sample-app.git'
            }
        }

        stage('Build Frontend (web-ui)') {
            steps {
                dir('web-ui') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Build Backend Services (Java)') {
            parallel {
                stage('Build Carts') {
                    steps {
                        dir('carts') {
                            sh './mvnw clean package -DskipTests'
                        }
                    }
                }

                stage('Build Orders') {
                    steps {
                        dir('orders') {
                            sh './mvnw clean package -DskipTests'
                        }
                    }
                }

                stage('Build Products') {
                    steps {
                        dir('products') {
                            sh './mvnw clean package -DskipTests'
                        }
                    }
                }

                stage('Build Checkout') {
                    steps {
                        dir('checkout') {
                            sh './mvnw clean package -DskipTests'
                        }
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Assumes your YAMLs are in k8s-manifests/
                sh 'kubectl apply -f k8s-manifests/'
            }
        }
    }

    post {
        failure {
            echo '❌ Build failed!'
        }
        success {
            echo '✅ Build and Deploy succeeded!'
        }
    }
}
