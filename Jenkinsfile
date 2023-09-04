pipeline {
    agent any 
    environment {
        PATH = "/usr/bin:$PATH"
        tag = "1.0"
        dockerHubUser="johnkalayu"
        containerName="insure-me"
        httpPort="8081"
    }
    stages {
        stage("code clone"){
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Johnkalayu/asi-insurance.git']])
            }
        }
        stage("Maven build"){
            steps {
                sh "mvn clean install -DskipTests"
            }
        }
        stage("Build Docker Image"){
            steps{
                sh "docker build -t johnkalayu/insure-me ."
            }
        }
        stage("push image to dockerhub"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: '$yourpasswd', usernameVariable: '$yourusername')]) {
                    sh "docker login -u $yourdockerhubuser -p $yourpasswd"
                    sh "docker push johnkalayu/insure-me:latest"
}
            }
        }
        stage("Docker container deployment"){
            steps{
                sh "docker rm insure-me -f"
                sh "docker pull johnkalayu/insure-me:latest"
                sh "docker run -d --name insure-me --rm -p 8081:8080 johnkalayu/insure-me:latest"
                echo "Application started on port: {8081} (8080)"
            }
        }
    }
}

