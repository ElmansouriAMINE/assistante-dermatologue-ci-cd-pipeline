pipeline{
    agent any
    tools{
        maven 'maven_3_5_4'
    }
    stages{
        stage('Build Maven project'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ElmansouriAMINE/assistante-dermatologue-ci-cd-pipeline']])
                bat 'mvnw -Pprod package verify jib:dockerBuild -DskipTests=true'
            }
        }
        stage('Build Docker image') {
            steps {
                echo 'Your image is already uploaded to Docker'
            }
        }
        stage('Push image to docker Hub'){
            steps{
                withCredentials([string(credentialsId: 'dockerhub-pwdd', variable: 'dockerhubpwd')]) {
                           bat 'docker login -u elmansouri01 -p ${dockerhubpwd}'
            }
            
                bat 'docker tag assistante-dermatologue:latest elmansouri01/assistante-dermatologue:latest'
                bat 'docker push elmansouri01/assistante-dermatologue:latest'
                        
                    
                }
            }
    
        stage('Run Docker Compose') {
            steps {
                bat 'docker-compose -f src/main/docker/app.yml up -d'
            }
        }
    }
}
