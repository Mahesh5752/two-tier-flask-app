pipeline {
    agent any

    stages {

        stage('Code') {
            steps {
                git url: 'https://github.com/Mahesh5752/two-tier-flask-app.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t two-tier-flask-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Developer/tester test likh ke dega'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker tag two-tier-flask-app "$DOCKER_USER/two-tier-flask-app:latest"
                    docker push "$DOCKER_USER/two-tier-flask-app:latest"
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose up -d --build flask-app'
            }
        }
    }
}
