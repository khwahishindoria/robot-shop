pipeline {
    agent any
    stages {
        stage('pulling robot-shopt') {
            steps {
                echo 'pulling...'
                sh '''
                cd /opt/jenkins/
                git pull https://github.com/khwahishindoria/robot-shop.git
                '''
            }
        }
        stage('Installing Helm') {
            steps {
                echo 'Installing Helm...'
                 sh '''
                 cd ~
                 curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
                 chmod 700 get_helm.sh
                 ./get_helm.sh
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deployment steps here
            }
        }
    }
}
