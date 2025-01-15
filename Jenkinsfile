pipeline {
    agent { label 'kubernetes' }
    stages {
        stage('pulling robot-shopt') {
            steps {
                echo 'pulling...'
                sh '''
                if [ -d /opt/jenkins/ ]
                then
                cd /opt/jenkins/robot-shop/
                git pull
                else
                git clone https://github.com/khwahishindoria/robot-shop.git
                fi
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
