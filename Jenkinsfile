pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-creds'
        DOCKER_IMAGE = 'afod/emailservice'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-creds', toolName: 'docker') {
                        def majorVersion = '1'
                        def buildNumber = env.BUILD_NUMBER.toInteger()
                        def formattedBuildNumber = String.format('%02d', buildNumber)
                        def imageTag = "${majorVersion}.${formattedBuildNumber}"
                        sh "docker build -t ${DOCKER_IMAGE}:${imageTag} ."
                    }
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-creds', toolName: 'docker') {
                        def majorVersion = '1'
                        def buildNumber = env.BUILD_NUMBER.toInteger()
                        def formattedBuildNumber = String.format('%02d', buildNumber)
                        def imageTag = "${majorVersion}.${formattedBuildNumber}"
                        sh "docker push ${DOCKER_IMAGE}:${imageTag}"
                    }
                }
            }
        }
        
        stage('Clean up disk') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-creds', toolName: 'docker') {
                        def majorVersion = '1'
                        def buildNumber = env.BUILD_NUMBER.toInteger()
                        def formattedBuildNumber = String.format('%02d', buildNumber)
                        def imageTag = "${majorVersion}.${formattedBuildNumber}"
                        sh "docker rmi ${DOCKER_IMAGE}:${imageTag}"
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
