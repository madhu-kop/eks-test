pipeline {
   environment {
    image = "madhu1718/test-repo"
    version = "1.0.0"
    NAMESPACE= "springboot-demoweb"
    RELEASE_NAME = "springboot-demoweb"
   region= "ap-south-1"
   cluster_name= "Cluster-avMYie8MkKMO"

   }
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Compile and package') {
      steps {
        sh """ cd springboot-demo/demoweb 
		mvn package 
		
		"""
      }
    }
 
  
    stage('Build Docker Image') {
      steps {
        script {
           sh """docker build -t $image:$version ./springboot-demo/demoweb/
                 $(aws ecr get-login --no-include-email --region us-east-2 | sed 's|https://||')
                docker push $image:$version
                docker rmi $image:$version

                   """

           }
		   
		   }
		   
		   }
		   
		   	stage('Deploy') {
       steps {
             sh """
	     echo " Update the kubeconfig"
	     aws eks --region $region update-kubeconfig --name $cluster_name
	     echo "Changing values file for built tag $version"
	     cd springboot-demo/demoweb/charts/springboot-demoweb
	     sed -i 's/imagetag/$version/' values.yaml
	     helm upgrade -n $NAMESPACE $RELEASE_NAME .
	     """
        }
    }
	
	}
	}
