pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('shell1') {
            steps {
                sh 'ls /etc/'
            }
        }
        stage('SH') {
            steps {
                sh 'ls /home/'
            }
        }
    }
}
