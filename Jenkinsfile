pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('4c04f4a2-1dfc-47dd-bd88-4c125c920b75') // Docker Hub credentials ID
        IMAGE_NAME = 'muhammadshoukat/scdlabexam' // Change to your Docker Hub username and image name
        GIT_REPO = 'https://github.com/NUCESFAST/scd-final-lab-exam-MuhammadSHOUKATSYED.git' // Change to your GitHub repository
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    def authDir = 'Auth'
                    def classroomDir = 'Classrooms'
                    def clientDir = 'client'
                    def eventBusDir = 'event-bus'
                    def postsDir = 'Posts'

                    dir(authDir) {
                        sh 'npm install'
                    }
                    dir(classroomDir) {
                        sh 'npm install'
                    }
                    dir(clientDir) {
                        sh 'npm install'
                    }
                    dir(eventBusDir) {
                        sh 'npm install'
                    }
                    dir(postsDir) {
                        sh 'npm install'
                    }
                }
            }
        }
        
        stage('Build Docker Images') {
            steps {
                script {
                    def services = ['Auth', 'Classrooms', 'client', 'event-bus', 'Posts']
                    
                    services.each { service ->
                        dir(service) {
                            sh "docker build -t ${IMAGE_NAME}-${service.toLowerCase()}:latest ."
                        }
                    }
                }
            }
        }
        
        stage('Push Docker Images') {
            steps {
                script {
                    def services = ['Auth', 'Classrooms', 'client', 'event-bus', 'Posts']
                    
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        services.each { service ->
                            sh "docker push ${IMAGE_NAME}-${service.toLowerCase()}:latest"
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
