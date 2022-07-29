pipeline {
    agent any
    stages {
        stage('Deploy backend') {
            steps {
                git 'https://github.com/QAFinalProject/spring-petclinic-rest.git'
                sh 'sudo docker run -d -p 9966:9966 springcommunity/spring-petclinic-rest'
                
            }
        }
        stage('Deploy frontend') {
            steps {
                git branch: 'angular11-pr', url: 'https://github.com/QAFinalProject/spring-petclinic-angular.git'
                sh '''sudo docker build -t spring-petclinic-angular:latest .
                sudo docker run --rm -p 8080:8080 spring-petclinic-angular:latest'''
            }
        }
        stage('Deploy nginx') {
            steps {
            sh 'ssh ubuntu@18.170.55.222 sudo docker run -d -p 80:80 --name nginx --mount type=bind,source=$(pwd)/nginx.conf,target=/etc/nginx/nginx.conf nginx'
            }
        }
    }
}