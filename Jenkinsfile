pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonar_scanner') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
                   nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexus_ID', groupId: 'SampleWebApp', nexusUrl: 'ec2-3-238-4-155.compute-1.amazonaws.com:9000', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            
        }
        
        stage('deploy to tomcat') {
          steps {
deploy adapters: [tomcat9(credentialsId: 'tomcat_ID', path: '', url: 'http://3.218.250.116:8080/')], contextPath: 'myapp', war: '**/*.war'              
              
          }
            
        }
            
        }
} 
