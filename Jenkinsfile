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
                sh 'sudo docker build -t my-front .'
            }
        }
        stage('-----checking created image-------') {
            steps {
                sh 'docker images'
                sh 'docker images'
            }
        }
        stage('-----start container----------') {
            steps {
                sh 'docker run -d -p 80:80 my-front'
            }
        }
    }
}
