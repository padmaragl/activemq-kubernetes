def createNamespace (namespace) {
    echo "Creating namespace ${namespace} if needed"
    sh "[ ! -z \"\$(kubectl get ns ${namespace} -o name 2>/dev/null)\" ] || kubectl create ns ${namespace}"
}
    
node {
   def mvnHome

   stage('Checkout') { // for display purposes
      // Get some code from a GitHub repository
      git branch: 'master',
        credentialsId: 'github-padmaragl',
        url: 'https://github.com/padmaragl/activemq-kubernetes.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M3'
   }
   
   stage('Dockerize Build'){
      sh "sudo docker build -t activemq-kubernetes ."
      sh "sudo docker tag activemq-kubernetes padmaraglaunchpad/activemq-kubernetes:latest"
      sh "sudo docker push padmaraglaunchpad/activemq-kubernetes:latest"
   }
   
   stage('Kubernetes Deploy') {
      //Kubernetes - create deployment and service
      
      sh "kubectl delete service activemq-kubernetes || true"
      sh "kubectl delete deployment activemq-kubernetes || true"
      
      sh "kubectl apply -f activemq_deployment.yaml"
      sh "kubectl apply -f activemq_service.yaml"
   }
  
      
}