pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('-----cstopping all----------') {
            steps {
                sh 'docker stop a4d7af8e67f0'
                sh 'docker stop 5a7846c0ff99'
                sh 'docker stop d05153d96915'
            }
        }
        stage('-----create docker network-------') {
            steps {
                sh 'docker network rm my_network'
                sh 'docker network create my_network'
                sh 'docker network ls'
            }
        }
        stage('-------move some frontend files---------') {
            steps {
                sh 'mv /var/lib/jenkins/workspace/aws-clone/FrontEnd/Dockerfile /var/lib/jenkins/workspace/aws-clone/FrontEnd/my-app/ '
            }
        }
        stage('----build image----') {
            steps {
                sh 'sudo docker build -t my-front /var/lib/jenkins/workspace/aws-clone/FrontEnd/my-app'
            }
        }
        stage('-----checking created image-------') {
            steps {
                sh 'docker images'
            }
        }
        stage('-----start container----------') {
            steps {
                sh 'docker run --network my_network -d -p 80:80 my-front'
            }
        }
        stage('-----start database image----------') {
            steps {
                sh 'docker run --network my_network -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Qwerty-1" -p 1433:1433 --name sql111 --hostname sql1 -d mcr.microsoft.com/mssql/server:2022-latest'
            }
        }
        stage('-----build back image----------') {
            steps {
                sh 'sudo docker build -t my-back /var/lib/jenkins/workspace/aws-clone/BackEnd/Amazon-clone'
            }
        }
        stage('-----start back container----------') {
            steps {
                sh 'docker run --network my_network -d -p 5034 my-back'
            }
        }
        stage('-----chacking----------') {
            steps {
                sh 'docker ps'
            }
        }
        
    }
}
