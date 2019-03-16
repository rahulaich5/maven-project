
        pipeline {
    agent any
            
     tools {
    maven 'localMaven'
      }
        

    parameters {
         string(name: 'tomcat_dev', defaultValue: '10.176.227.167', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '10.176.226.165', description: 'Production Server')
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
                        sh "scp -i /root/downloads/certs/tomcat-demo.pem **/target/*.war /var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /root/downloads/certs/tomcat-demo.pem **/target/*.war centos@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
