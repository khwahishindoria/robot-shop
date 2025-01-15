pipeline {
    agent { label 'kubernetes' }
    stages {
        stage('pulling robot-shopt') {
            steps {
                echo 'pulling...'
                sh '''
                cd /opt/jenkins/
                git clone https://github.com/khwahishindoria/robot-shop.git
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
        stage('Deploying robot-shop') {
            steps {
                echo 'Deploying robot-shop...'
                 sh '''
                 cd /opt/jenkins/robot-shop/K8s/helm
                 kubectl create ns robot-shop
                 helm install robot-shop --namespace robot-shop /opt/jenkins/robot-shop/K8s/helm/
                 kubectl apply -f /opt/jenkins/robot-shop/pv.yml                 
                '''            }
        }
        stage('Deploying EFK Stack for monitoring') {
            steps {
                echo 'Deploying EFK Stack for monitoring'
                 sh '''
                 cd /opt/jenkins/robot-shop/EFK
                 kubectl create ns logging
                 kubectl apply -f .               
                '''            }
        }
    }
}
