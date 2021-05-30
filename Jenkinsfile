pipeline {
    agent any
    

    stages {
        stage('Test') {
            steps {
                git 'https://github.com/roeehersh/nginx-assignment.git'
                echo "current build number: ${currentBuild.number}"
                dir("nginx") {
                    sh "docker build -t roeehersh/nginx-assignment:${currentBuild.number} --build-arg JENKINS_BUILD_NO=${currentBuild.number} ./"
                    withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docker login --username $USERNAME --password $PASSWORD"
                        sh "docker push roeehersh/nginx-assignment:${currentBuild.number}"
                    }
                }
                sh "kubectl set image deployment/nginx-assignment nginx=roeehersh/nginx-assignment:${currentBuild.number} --record"
            }
        }
    }
}