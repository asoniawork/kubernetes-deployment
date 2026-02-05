pipeline {
    agent any
    
    stages {
        stage('Git Clone') {
            steps {
                echo 'Cloning repository from GitHub...'
                git branch: 'main', 
                    url: 'https://github.com/asoniawork/kubernetes-deployment.git'
                echo 'Repository cloned successfully!'
            }
        }
        
        stage('Display Files') {
            steps {
                echo 'Listing files in workspace...'
                sh 'ls -la'
                sh 'cat deployment.yaml'
                sh 'cat service.yaml'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh '''
                    export KUBECONFIG=$HOME/.kube/config
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
