pipeline {
    agent any
    stages{
        stage('build project'){
            steps{
                git url:'https://github.com/indusharma19/star-agile-banking-finance.git', branch: "master"
                sh 'mvn clean compile'
                sh 'mvn test'
                sh 'mvn package'
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t finance-me-bank .'
                    sh 'docker images'
                }
            }
        }  
        
        stage('dockerhub login') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'dockerpass')]) {
                sh 'docker login -u indu1919 -p ${dockerpass}'
                }
            }
        }

        stage('tag and push image'){
            steps{
                script{
                    sh 'docker tag finance-me-bank indu1919/finance-me-bank:v1'
                    sh 'docker push indu1919/finance-me-bank:v1'
                }
            }
        }  
    }
}