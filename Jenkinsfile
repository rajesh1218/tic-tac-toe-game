pipeline{
    agent any
    stages{

        stage ("Build App Image") {
            steps {
                script {
                
                    // Build Docker image
                    sh "docker build -t ${REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER} ."
                }
            }
        }
        

        stage ("Push App Image") {
            steps {
              
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh """
                       echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                       docker push ${REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER}
                    """
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