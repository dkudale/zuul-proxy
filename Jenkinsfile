pipeline {
	
   agent none
	
    stages {
	  
        stage('build') {
	    	agent { 
	    		docker { 
	    					image 'maven:3.3.3' 
	    					args '-v $HOME/.m2:/root/.m2'
	    			} 
	    	}  	
		
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh "mvn clean install"
            }
        }
        
        stage('Make Container') {
            agent any
		    steps {
				sh "docker build -t zuul-proxy:${env.BUILD_ID} ."
				sh "docker tag zuul-proxy:${env.BUILD_ID} gcr.io/appsutility-141503/zuul-proxy:${env.BUILD_ID}"
				sh "gcloud docker -- push gcr.io/appsutility-141503/zuul-proxy:${env.BUILD_ID}"
		    }
	    }
    	
    }
     
	post { 
        always { 
            echo 'Completed'
        }
	}     
    
}
