pipeline {
    
    agent any
    tools {
        maven 'maven-3.9'
    }
    
    parameters {
     string defaultValue: 'ec2-18-215-158-115.compute-1.amazonaws.com', description: 'Remote staging server', name: 'staging_server'
    }

    
    stages{
        stage ('checkout code'){
            steps{
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/stawssthub/webapp.git']])
            }
        }

        stage ('mvn test) {
               steps {
                   sh 'mvn test'
               }
        }
        
        stage ('build cod') {
            steps{
                sh 'mvn clean package'
            }
            post {
                success{
                echo 'Archiving the artifacts'
                archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        
        stage ('deploy to tomcat server') {
            steps {
                 
                 deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://ec2-18-215-158-115.compute-1.amazonaws.com:8090')], contextPath: 'webapps', war: '**/*.war'
                 //sh "scp -v -o StrictHostKeyChecking=no **/*.war root@${params.staging_server}:/opt/tomcat/webapps/"
            }
        }
    }
}
