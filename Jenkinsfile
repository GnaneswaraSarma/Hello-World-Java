pipeline 
{
    agent any
    	tools {
	    maven 'localMaven'
	      }
        stages
   {
        
        stage('Build')
        {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging')
        {
           steps 
           {
             build job: 'deploy_to_staging'                         
           }
        }
        stage('Deploy to Prod')
        {
            steps
            {
                timeout(time:5, unit: 'DAYS')
                {
                    input message:'Approve Production Deployment?'
                }
                build job: 'deploy_to_prod'
            }
            post{
                success{
                    echo 'Code deployed to production'
                }
                failure {
                    echo 'Deployment failed'
                }
            }
        }
        
        
    }
}