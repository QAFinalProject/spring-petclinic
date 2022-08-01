pipeline {
    agent any
    stages {
        stage('Clone down functional files') {
            steps {
                git branch: 'main', url: 'https://github.com/QAFinalProject/spring-petclinic.git'
                sh 'sudo apt update'
                sh 'scp -i ~/.ssh/aws-key-london.pem /home/ubuntu/spring-petclinic/docker-check-script.sh ubuntu@3.10.246.212:/home/ubuntu/'   
            }
        } 

        stage('Deploy backend') {
            steps {              
                git 'https://github.com/QAFinalProject/spring-petclinic-rest.git'
                sh '''ssh -i /home/jenkins/.ssh/aws-key-london.pem ubuntu@3.10.246.212 sudo chmod +x docker-check-script.sh
                ssh -i /home/jenkins/.ssh/aws-key-london.pem ubuntu@3.10.246.212 ./docker-check-script.sh'''
            }
        }
        stage('Deploy frontend') {
            steps {
                git branch: 'master', url: 'https://github.com/QAFinalProject/spring-petclinic-angular.git'
                sh '''sudo docker build -t spring-petclinic-angular:latest .
                ssh -i /home/jenkins/.ssh/aws-key-london.pem ubuntu@3.10.246.212 sudo docker run --rm -d --name frontend -p 8080:8080 spring-petclinic-angular:latest'''
            }
        }
        stage('Deploy nginx') {
            steps {
            sh 'ssh -i /home/jenkins/.ssh/aws-key-london.pem ubuntu@18.134.141.28 sudo docker run -d -p 80:80 --name nginx --mount type=bind,source=$(pwd)/nginx.conf,target=/etc/nginx/nginx.conf nginx'
            }
        }
    }
}