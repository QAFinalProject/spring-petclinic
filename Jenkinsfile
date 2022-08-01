pipeline {
    agent any
    stages {
        stage('Clone down functional files') {
            steps {
                sh '''cd ~
                rm -rf sping-petclinic
                git clone https://github.com/QAFinalProject/spring-petclinic.git'''
            }
        }
        
        stage('Deploy backend') {
            steps {
                
                git 'https://github.com/QAFinalProject/spring-petclinic-rest.git'
                sh 'cd ~/spring-petclinic && sudo chmod +x docker-check-script.sh'
                sh './docker-check-script.sh'
            }
        }
        stage('Deploy frontend') {
            steps {
                git branch: 'master', url: 'https://github.com/QAFinalProject/spring-petclinic-angular.git'
                sh '''sudo docker build -t spring-petclinic-angular:latest .
                sudo docker run --rm -d --name frontend -p 8080:8080 spring-petclinic-angular:latest'''
            }
        }
        stage('Deploy nginx') {
            steps {
            sh 'ssh ubuntu@18.170.55.222 sudo docker run -d -p 80:80 --name nginx --mount type=bind,source=$(pwd)/nginx.conf,target=/etc/nginx/nginx.conf nginx'
            }
        }
    }
}