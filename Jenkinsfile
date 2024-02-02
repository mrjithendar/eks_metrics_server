pipeline {

    agent any

    environment {
        clusterName = "roboshop-eks-cluster-demo"
        awsRegion = "us-east-1"
    }

    stages {
        stage('Import EKS Cluster') {
            steps {
                withAWS(credentials: 'awsCreds', region: 'us-east-1') {
                    sh "aws eks update-kubeconfig --region ${awsRegion} --name ${clusterName}"
                }
            }
        }
        stage('Deploy Metrics Server') {
            steps {
                withAWS(credentials: 'awsCreds', region: 'us-east-1') {
                    // for more details visit: https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html
                    sh "kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml"
                }
            }
        }
        stage('Metrics Server Status') {
            steps {
                withAWS(credentials: 'awsCreds', region: 'us-east-1') {
                    sh "sleep 10"
                    sh "kubectl get deployment metrics-server -n kube-system"
                }
            }
        }
    }
    options {
        preserveStashes()
        timestamps()
    }
    post {
        always {
            cleanWs()
        }
    }
}
