pipeline{
    agent any
    stages{

       stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build -t tic:latest ."
                       sh "docker tag tic:latest ash425/tic:latest "
                       sh "docker push ash425/tic:latest "
                    }
                }
            }
        }

        stage ("Deploy to cluster dev-kt-k8s") {
            steps {
                withKubeConfig(credentialsId: 'kubeconfig-dev-kt-k8s') {
                    sh "kubectl apply -f kubernetes/deployment.yaml "
                    sh "kubectl apply -f kubernetes/service.yaml"
                    sh "kubectl apply -f kubernetes/ingress.yaml"
                }
            }
        }
    }
}