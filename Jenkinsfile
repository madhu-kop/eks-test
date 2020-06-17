pipeline {

  agent {
    node {
      label 'master'
    }

  }
properties([parameters([string(defaultValue: '1.0.0', description: 'Please provide the version number', name: 'version', trim: false)])])
   environment {
    image = "madhu1718/test-repo"
    version = "${params.version}"
    region= "ap-south-1"
    cluster_name= "Cluster-Qp97W0J0sYeB"
	

   }
  stages {
    stage('Compile and package') {
      steps {
        sh "mvn package"
      }
    }
 
  
    stage('Build Docker Image') {
      steps {
        script {
           sh """docker build -t $image:$version .
               docker push $image:$version
                docker rmi $image:$version
				"""
				}
				}
		 }
		   
	stage('Deploy into K8S') {
       steps {
             sh """
	     echo " Update the kubeconfig"
	     aws eks --region $region update-kubeconfig --name $cluster_name
	     echo "Changing image name and version in deployment yaml file"
	     imagefullname=$image:$version
	     sed -i 's/imagefullname/$imagefullname/' deployment.yaml
	           """
        }
    }
	
	}
	}
