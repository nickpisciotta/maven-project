pipeline {
    agent any

    parameters {
         string(name: 'test_staging', defaultValue: '54.227.57.112', description: 'Staging Server')
         string(name: 'test_prod', defaultValue: '54.174.3.103', description: 'Production Server')
    }

    tools{
        maven 'localMaven'
    }

    triggers {
         pollSCM('* * * * *')
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
                        sh "scp -i /Users/npisciot/ThoughtWorks/tutorials/jenkins_practice/tomcat-demo.pem **/target/*.war ec2-user@${params.test_staging}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/npisciot/ThoughtWorks/tutorials/jenkins_practice/tomcat-demo.pem ec2-user@${params.test_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}