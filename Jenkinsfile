pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.15.117.92', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.218.149.199', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }
     
     tools {
        maven 'localMaven'
     }

stages{
        stage('Build'){
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /Users/kobe-c/Documents/Jenkins_Maven/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/kobe-c/Documents/Jenkins_Maven/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}