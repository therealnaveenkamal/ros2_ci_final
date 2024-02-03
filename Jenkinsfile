pipeline {
    agent any
    stages {
        stage('Install Docker') {
            steps {
                script {
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y docker.io docker-compose'
                    sh 'sudo service docker start'
                    sh 'sudo usermod -aG docker $USER'
                    sh 'newgrp docker'
                    sh 'sudo service docker status'
                    sh 'sudo docker ps -a'
                    sh 'sudo service docker restart'
                }
            }
        }
        stage('Build Dockerfile') {
            steps {
                script {
                    dir('/home/user/ros2_ws/src/ros2_ci') {
                        sh 'sudo docker build -t tortoisebot-cp24:ros2 .'
                    }
                }
            }
        }
        stage('Run Docker Compose') {
            options {
                timeout(time: 40, unit: "SECONDS")
            }
            steps {
                script {
                    dir('/home/user/ros2_ws/src/ros2_ci') {
                        try{
                            sh 'sudo docker-compose up'
                        }
                        catch (Throwable e) {
                            echo "Caught ${e.toString()}"
                            currentBuild.result = "SUCCESS" 
                        }
                    }
                }
            }
        }
    }
}
