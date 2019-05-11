pipeline 
{
    agent any
    	tools {
	    maven 'localMaven'
	      }
    parameters 
       {
             string(name: 'stg_one', defaultValue: '13.234.112.34', description: 'Staging Server')
             string(name: 'prd_one', defaultValue: '13.233.106.103', description: 'Production Server')
        }
    
        triggers
        {
             pollSCM('* * * * *')
         }
    
    stages{
            stage('Build')
            {
                steps 
                {
                    sh 'mvn clean package'
                }
                post 
                {
                    success {
                        echo 'Now Archiving...'
                        archiveArtifacts artifacts: '**/target/*.war'
                    }
                }
            }
    
            stage ('Deployments'){
                parallel{
                    stage ('Deploy to Staging'){
                        steps {
                            sh "scp -i /var/lib/jenkins/acloudgurumykeypair.pem /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.stg_one}:/var/lib/tomcat/webapps"
                        }
                    }
    
                    stage ("Deploy to Production"){
                        steps {
                            sh "scp -i /var/lib/jenkins/acloudgurumykeypair.pem /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.prd_one}:/var/lib/tomcat/webapps"
                        }
                    }
                }
            }
    }
       
}