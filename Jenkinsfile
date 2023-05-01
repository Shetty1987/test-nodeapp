pipeline{
    agent { label 'worker' }   
    options {
        buildDiscarder(logRotator(numToKeepStr: '1'))
    }
    parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }
    triggers { cron('H */4 * * 1-5') 
    pollSCM('H */4 * * 1-5')
    }
    stages{
        stage('build')
        {
            steps{
                sh """
                cat Dockerfile
                docker build -t 724012784310.dkr.ecr.us-east-1.amazonaws.com/node-app:v${BUILD_NUMBER} .
                aws ecr get-login-password --region us-east-1 |docker login --username AWS --password-stdin 724012784310.dkr.ecr.us-east-1.amazonaws.com
                docker push 724012784310.dkr.ecr.us-east-1.amazonaws.com/node-app:v${BUILD_NUMBER}
                 """
            }
        }
        stage('deploy')
        {
            steps{
                    sshagent(credentials:['Login_App']){
                      
                        sh """
                        ssh -o  StrictHostKeyChecking=no ubuntu@10.0.1.198 
                        hostname
                        aws ecr get-login-password --region us-east-1 |docker login --username AWS --password-stdin 724012784310.dkr.ecr.us-east-1.amazonaws.com 
                        docker run -d -p 8081:8081 --name nodeapp 724012784310.dkr.ecr.us-east-1.amazonaws.com/node-app:v${BUILD_NUMBER}
                        """
                   }
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello  again!'
        }
    }
}
