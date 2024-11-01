pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
 /*       stage('-----create docker network-------') {
            steps {
                sh 'docker network rm my_network'
                sh 'docker network create my_network'
                sh 'docker network ls'
            }
        }
*/
                stage('Create Docker Network') {
            steps {
                script {
                    // Перевіряємо, чи існує мережа
                    def networkExists = sh(script: "docker network ls --filter name=my_network -q", returnStdout: true).trim()
                    
                    // Якщо мережа існує, видаляємо її
                    if (networkExists) {
                        echo 'Network exists. Deleting...'
                        sh 'docker network rm my_network'
                    } else {
                        echo 'Network does not exist. Creating...'
                    }
                    
                    // Створюємо мережу
                    sh 'docker network create my_network'
                    
                    // Перевіряємо список мереж
                    sh 'docker network ls'
                }
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
        /*stage('-----start container----------') {
            steps {
                sh 'docker run --network my_network -d -p 80:80 my-front'
            }
        }
        */
        /*
        stage('-----start database image----------') {
            steps {
                sh 'docker run --network my_network -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Qwerty-1" -p 1433:1433 --name sql111 --hostname sql1 -d mcr.microsoft.com/mssql/server:2022-latest'
            }
        }
        */
        stage('-----build back image----------') {
            steps {
                sh 'sudo docker build -t my-back /var/lib/jenkins/workspace/aws-clone/BackEnd/Amazon-clone'
            }
        }
        /*
        stage('-----start back container----------') {
            steps {
                sh 'docker run --network my_network -d -p 5034 my-back'
            }
        }
        */
        stage('-----chacking----------') {
            steps {
                sh 'docker ps'
            }
        }
        stage("docker login") {
            steps {
                echo " ============== docker login =================="
                withCredentials([usernamePassword(credentialsId: 'test123', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        def loginResult = sh(script: "docker login -u $USERNAME -p $PASSWORD", returnStatus: true)
                        if (loginResult != 0) {
                            error "Failed to log in to Docker Hub. Exit code: ${loginResult}"
                        }
                    }
                }
                echo " ============== docker login completed =================="
            }
        }

        stage("docker push") {
            steps {
                sh 'echo " ============== pushing image =================="'
                // Відправлення образів на Docker Hub
                //docker push your_dockerhub_username/my-front:latest
                //docker push your_dockerhub_username/my-back:latest
                //docker push your_dockerhub_username/mssql-server:latest

                // Тегування образів як частини одного проєкту
                sh 'docker tag my-front dimax555/web-app:frontend'
                sh 'docker tag my-back dimax555/web-app:backend'
                sh 'docker tag mcr.microsoft.com/mssql/server:2022-latest dimax555/web-app:database'

                // Завантаження образів на Docker Hub
                sh 'docker push dimax555/web-app:frontend'
                sh 'docker push dimax555/web-app:backend'
                sh 'docker push dimax555/web-app:database'
                
            }
        }
        
    }
}
