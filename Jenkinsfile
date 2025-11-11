pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'na5ko/tp3_devops'
        HELM_CHART_PATH = './tp3'
    }

    stages {

        stage('Construire l\'image Docker') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Pousser l\'image Docker') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Déployer avec Helm') {
            steps {
                sh 'helm upgrade --install mon-app $HELM_CHART_PATH'
            }
        }
    }
}
