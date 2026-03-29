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

        stage("testing") {
            steps {
                echo "testing successfully."
            }
        }

        stage("push to docker hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubcreds',   // Must match Jenkins credentials ID
                    usernameVariable: 'DOCKER_USER', 
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        docker login -u $DOCKER_USER -p $DOCKER_PASS
                        docker tag my-app:latest $DOCKER_USER/my-app:latest
                        docker push $DOCKER_USER/my-app:latest
                    '''
                }
            }
        }

        stage("deploy") {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d --build'
            }
        }
    }
}
