pipeline {
    agent any

    stages {
        stage("code") {
            steps {
                git url: 'https://github.com/shahidmustafa695-stack/flask-app.git', branch: 'main'
            }
        }

        stage("build") {
            steps {
                sh 'docker build -t my-app:latest .'
            }
        }

        stage("push to docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {
                    sh 'docker login -u $dockerHubUser -p $dockerHubPass'
                    sh 'docker tag my-app:latest $dockerHubUser/my-app:latest'
                    sh 'docker push $dockerHubUser/my-app:latest'
                }
            }
        }

        stage("deploy") {
            steps {
                sh '''
                    docker-compose down
                    docker-compose up -d
                '''
            }
        }
    }
}
