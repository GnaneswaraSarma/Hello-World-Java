pipeline 
{
    agent any
    	tools {
	    maven 'localMaven'
	      }
    parameters 
       {
             string(name: 'tomcat_dev', defaultValue: '13.233.69.241', description: 'Staging Server')
             string(name: 'tomcat_prod', defaultValue: '13.127.28.241', description: 'Production Server')
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
                            sh "scp -i /var/lib/jenkins/acloudgurumykeypair.pem /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                        }
                    }
    
                    stage ("Deploy to Production"){
                        steps {
                            sh "scp -i /home/jenkins/acloudgurumykeypair.pem /var/lib/jenkins/workspace/FullyAutomated/webapp/target//*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                        }
                    }
                }
            }
    }
       
}