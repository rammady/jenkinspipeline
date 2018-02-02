pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
			post {
			    success {
				    echo 'Now Archiving...'
				    archiveArtifacts artifacts: '**/target/*.war'
				}
            }
        }
	stage ('Deploy to staging'){
	   steps{
	      build job: 'deploy-to-staging'
	   }
	    } 
	    
	    stage ('Deploy To Production'){
		    steps{
			    timeout(time:5, unit :'DAYS'){
			          inpput message: 'Approve Production Deployment?'
			    }
			    
			    build job: 'deploy-to-prod'
		    }
		    post{
			    success{
			     echo 'Code Deployed to Prodution.'
			    }
			    
			    failure{
			       echo 'Deployment failed'
			    }
		    }
	    }
	    
    }
}
