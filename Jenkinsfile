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
                sh "docker build -t {johnkalayu}/insure-me"
            }
        }
        stage("push image to dockerhub"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PAPILLON@321', usernameVariable: 'johnkalayu')]) {
                    sh "docker login -u johnkalayu -p PAPILLON@321"
                    sh "docker push johnkalayu/insure-me:"
}
            }
        }
        stage("Docker container deployment"){
            steps{
                sh "docker rm insure-me -f"
                sh "docker pull johnkalyu/insure-me:"
                sh "docker run -d --rm -p 8080:8091 --name insure-me $johnkalayu/insure-jo:"
                echo "Application started on port: {8080} (8081)"
            }
        }
    }
}

