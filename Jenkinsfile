pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.189.31.183', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.191.219.141', description: 'Production Server')
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
                        sh "whoami"
                        sh "scp -i /home/aponcepe/webserver/aws/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "whoami"
                        sh "scp -i /home/aponcepe/webserver/aws/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}
