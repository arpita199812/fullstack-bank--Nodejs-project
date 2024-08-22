pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS-Access-key')
        AWS_SECRET_ACCESS_KEY = credentials('AWS-Access-key')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/arpita199812/fullstack-bank--Nodejs-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-nodejs-app1')
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    docker.image('my-nodejs-app1').inside {
                        bat 'npm test'
                    }
                }
            }
        }
        stage('Package Application') {
            steps {
                script {
                    docker.image('my-nodejs-app1').inside {
                        bat 'npm run build'
                    }
                }
            }
        }
        stage('Upload to S3') {
            steps {
                script {
                    docker.image('my-nodejs-app1').inside {
                        withAWS(region: 'us-east-1', credentials: 'AWS-Access-key') {
                            s3Upload(bucket: 'mynodejs-s3 ', path: 'build/*')
                        }
                    }
                }
            }
        }
        stage('Deploy to Elastic Beanstalk') {
            steps {
                script {
                    docker.image('my-nodejs-app1').inside {
                        withAWS(region: 'us-east-1', credentials: 'AWS-Access-key') {
                            bat 'eb deploy'
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
