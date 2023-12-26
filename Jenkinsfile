pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                git url:'https://github.com/akshu20791/DevOpsClassCodes/', branch: "master"
            }
        }
        stage('Build') {
            steps {
               sh "mvn clean package"
            }
        }
       
        stage('Build Image') {
            steps {
                sh 'docker build -t akshatimg .'
                sh 'docker tag akshatimg:latest akshu20791/akshatimgaddbook:latest'
            }
        }
        stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockercred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push santhoshrb/akshatimgaddbook:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run -itd --name My-first-containe211 -p 80:8082 akshu20791/akshatimgaddbook:latest'
                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.20.232 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
