pipeline {
    agent any
    

    stages {
        stage('Build') {
            steps {
                dir("nginx") {
                    sh "docker build -t roeehersh/nginx-assignment:${currentBuild.number} --build-arg JENKINS_BUILD_NO=${currentBuild.number} ./"
                    withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docker login --username $USERNAME --password $PASSWORD"
                        sh "docker push roeehersh/nginx-assignment:${currentBuild.number}"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh "kubectl set image deployment/nginx-assignment nginx=roeehersh/nginx-assignment:${currentBuild.number} --record"
            }
        }
    }
}