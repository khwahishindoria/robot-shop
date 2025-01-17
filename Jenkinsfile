pipeline {
    agent any
    stages {
        stage('pulling robot-shopt') {
            steps {
                echo 'pulling...'
                sh '''
                if [ -d /tmp/jenkins/robot-shop/ ]
                then
                cd /tmp/jenkins/robot-shop/
                git pull
                else
                mkdir -p /tmp/jenkins/
                cd /tmp/jenkins/
                git clone https://github.com/khwahishindoria/robot-shop.git
                fi
                '''
            }
        }
        stage('Deploying robot-shop') {
            steps {
                echo 'Deploying robot-shop...'
                 sh '''
                 cd /tmp/jenkins/robot-shop/K8s/helm
                 kubectl create ns robot-shop
                 helm install robot-shop --namespace robot-shop /opt/jenkins/robot-shop/K8s/helm/
                 kubectl apply -f /tmp/jenkins/robot-shop/pv.yml
                '''            }
        }
        stage('Deploying EFK Stack for monitoring') {
            steps {
                 echo 'Deploying EFK Stack for monitoring'
                 sh '''
                 cd /tmp/jenkins/robot-shop/EFK
                 kubectl create ns logging
                 kubectl apply -f Namespace.yml
                 kubectl apply -f .
                '''            }
        }
    }
}
