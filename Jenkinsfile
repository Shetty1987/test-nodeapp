pipeline{
    agent any    
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
                sh "cat Dockerfile"
                sh "sudo docker build -t 724012784310.dkr.ecr.us-east-1.amazonaws.com/node-app:v${BUILD_NUMBER} ."
                sh "aws ecr get-login-password --region us-east-1 |sudo docker login --username AWS --password-stdin 724012784310.dkr.ecr.us-east-1.amazonaws.com"
                sh "sudo docker push 724012784310.dkr.ecr.us-east-1.amazonaws.com/node-app:v${BUILD_NUMBER}"
            }
        }
        stage('deploy')
        {
            steps{
                sh "sleep 10"
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
        }
    }
}