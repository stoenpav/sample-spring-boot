pipeline {
    agent any
    tools{
        nodejs 'node'
        docekrTool 'docker'
    }
    triggers{
        githubPush()
    }

    environment{
        DOCKERHUB_CREDENTIALS=credentials('my_docker_cred')
        IMAGE_NAME='seeshellol/samplespringboot'
    }
    
    stages {
        stage('Clean'){
            steps{
                cleanWs()
            }
        }
        stage('Clone Repo'){
            steps{
               git branch: 'master', url: 'https://github.com/seeshellol/sample-spring-boot.git'
            }
            }
        
        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('Docker login') {
            steps {
                sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
            }
        }

          stage('Docker build and tag'){
            steps {
                sh 'docker build -t ${IMAGE_NAME} -f Dockerfile .'
                sh 'docker tag ${IMAGE_NAME} ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

	stage('Docker Push'){
            steps {
              sh 'docker push ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }
        
        
    }
}
