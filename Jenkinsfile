pipeline {
    agent { label 'java' }
    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Git Clone') {
            steps {
                echo "Cloning GitHub repo..."
                checkout scm
            }
        }

        stage('Precheck') {
            steps {
                sh "kubectl version --client=true"
                sh "kubectl cluster-info"
                sh "kubectl get nodes"
            }
        }

        stage('Dry run') {
            steps {
                sh '''
                  kubectl apply --dry-run=client -f deployment.yaml
                  kubectl apply --dry-run=client -f service.yaml
                '''
            }
        }

        stage('Run Kubernetes Manifests') {
            steps {
                sh "kubectl get pods"
                sh "kubectl get rs"
                sh "kubectl get deployment"
                echo "Applying Kubernetes YAML files..."
                sh '''
                  kubectl apply -f deployment.yaml
                  kubectl apply -f service.yaml
                '''
            }
        }

        stage('Rollout status') {
            steps {
                sh "kubectl rollout status deployment/nginx-deploy"
            }
        }

        stage('verification') {
            steps {
                sh '''
                  for i in 1 2 3 4 5; do
                    curl -sf 192.168.49.2:30303 && exit 0
                    sleep 2
                  done
                  exit 1
                '''
            }
        }
    }
}
