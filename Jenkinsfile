pipeline {
    agent any
    environment {
        DOCKER_REPOSITORY = "hardik794/devops-task"
        VERSION = "1.2.1"
    }
    stages {
        stage('Build Docker image') {
            steps {
                sh "docker build -t ${DOCKER_REPOSITORY}:${env.BRANCH_NAME}-${VERSION} ."
            }
        }
        stage('Push Docker image') {
            steps {
                withCredentials(
                    [
                        usernamePassword(
                            credentialsId: 'docker-hub',
                            passwordVariable: 'DOCKER_HUB_LOGIN_PSW',
                            usernameVariable: 'DOCKER_HUB_LOGIN_USR'
                            )
                        ]
                    ) {
                        sh "docker login -u ${DOCKER_HUB_LOGIN_USR} -p ${DOCKER_HUB_LOGIN_PSW}"
                        sh "docker push ${DOCKER_REPOSITORY}:${env.BRANCH_NAME}-${VERSION}"
                }
            }
        }
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}
