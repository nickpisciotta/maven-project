pipeline {
    agent any
    tools{
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
        stage('Deploy to Staging'){
           steps {
               build job: 'Deploy-to-staging'
           }
        }
        stage ('Deploy to Production'){
            steps{
                timemout(time: 5, unit: 'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }

                build job: 'Deploy-to-production'
            }
            post {
                success {
                    echo 'Code deployed to production'
                }

                failure {
                    echo 'Deployment failed'
                }
            }
        }

    }
}