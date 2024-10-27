pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('----build image----') {
            steps {
                sh 'docker build -t my-front /var/lib/jenkins/workspace/aws-clone/Dockerfile]'
            }
        }
        stage('-----checking created image-------') {
            steps {
                sh 'docker images'
                sh 'docker imahes | grep my-front'
            }
        }
        stage('-----start container----------') {
            steps {
                sh 'docker run -d -p 80:80 my-front'
            }
        }
    }
}
