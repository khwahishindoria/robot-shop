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
                 var=`kubectl get ns | grep robot-shop | wc -l`
                 if [[ "$var" == "1" ]] 
                 then
                 echo "service already deployed"
                 else
                 cd /tmp/jenkins/robot-shop/K8s/helm
                 kubectl create ns robot-shop
                 helm install robot-shop --namespace robot-shop /opt/jenkins/robot-shop/K8s/helm/
                 kubectl apply -f /tmp/jenkins/robot-shop/pv.yml
                 fi
                '''            }
        }
        stage('Deploying EFK Stack for monitoring') {
            steps {
                echo 'Deploying EFK Stack for monitoring'
                 sh '''
                 var2=`kubectl get ns | grep robot-shop | wc -l`
                 if [[ "$var2" == "1" ]
                 then
                 echo "Monitoring already deployed" 
                 else
                 cd /tmp/jenkins/robot-shop/EFK
                 kubectl create ns logging
                 kubectl apply -f .
                 fi
                '''            }
        }
    }
}
