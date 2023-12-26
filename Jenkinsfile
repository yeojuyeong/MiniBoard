pipeline {
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_TOKEN_JENKINS')
    }
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage("Compile") {
            steps {
                sh "./gradlew compileJava"
            }
        }
        stage("Build") {
           steps {
               sh "./gradlew build"
               sh "cp /var/jenkins_home/MiniBoard/build/libs/MiniBoard-0.0.1-SNAPSHOT.jar /var/jenkins_home/MiniBoard/docker/MiniBoard/"
           } 
        }
        stage("Docker Authorizing") {
           steps {
               sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin" 
           }
        }
        stage("Apache Docker Image Build & Push") {
           steps {
               sh "docker build -t redleon1/apache2_sbBoard:$BUILD_NUMBER" /var/jenkins_home/docker/MiniBoard/"
               sh "docker push redleon1/apache2_smBoard:$BUILD_NUMBER"
           }
        }
        stage("MiniBoard Docker Image Build & Push") {
           steps {
               sh "docker build -t redleon1/MiniBoard_smBoard:$BUILD_NUMBER" /var/jenkins_home/docker/MiniBoard/"
               sh "docker push redleon1/MiniBoard_smBoard:$BUILD_NUMBER" 
           } 
        }
        stage("Mariadb Docker Image Build & Push") {
           steps {
               sh "docker build -t redleon1/mariadb_smBoard:$BUILD_NUMBER" /var/jenkins_home/docker/MiniBoard/"
               sh "docker push redleon1/mariadb_smBoard:$BUILD_NUMBER"
           }
        }
    }
}
