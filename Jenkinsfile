pipeline {
    agent any
    stages {
        stage('Build Project') {
            steps {
                git url: 'https://github.com/indusharma19/star-agile-banking-finance.git', branch: 'master'
                sh 'mvn clean compile'
                sh 'mvn test'
                sh 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t finance-me-bank .'
                    sh 'docker images'
                }
            }
        }

        stage('DockerHub Login') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'dockerpass')]) {
                    sh 'docker login -u indu1919 -p ${dockerpass}'
                }
            }
        }

        stage('Tag and Push Image') {
            steps {
                script {
                    sh 'docker tag finance-me-bank indu1919/finance-me-bank:v1'
                    sh 'docker push indu1919/finance-me-bank:v1'
                }
            }
        }

        stage('Clone Repository for Ansible') {
            steps {
                // Clone the repository to get ansible-playbook.yml and inventory file
                git url: 'https://github.com/indusharma19/star-agile-banking-finance.git', branch: 'master'
            }
        }

        stage('Add Host Key') {
            steps {
                script {
                    sh 'ssh-keyscan -H 172.31.24.94 >> ~/.ssh/known_hosts'
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible', keyFileVariable: 'KEYFILE')]) {
                    script {
                        sh '''
                            ansible-playbook -i inventory ansible-playbook.yml --key-file $KEYFILE
                        '''
                    }
                }
            }
        }
    }
}
