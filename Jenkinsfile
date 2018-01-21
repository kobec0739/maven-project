pipeline {
    agent any
    
    tools {
        maven 'localMaven'
    }
    
    stages {
        stage('Build') {
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
        
        stage('Deploy to Staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }
        
        stage('Deploy to PROD') {
            steps {
                timeout(time:5, unit:'Days'){
                    input message: 'Approve PRODUCTION Deployment'
                }
                
                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Prod'
                }
                
                failure {
                    echo 'Deployment failed'
                }
            }
        }

    }
}