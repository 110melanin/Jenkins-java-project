pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout') {
            
            steps {
                
                script {
                    
                    git branch: 'main', url: 'https://github.com/110melanin/Jenkins-java-project.git'
                }
            }
        }
        stage('Unit Testing') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Integration Testing') {
            steps {
                sh 'mvn verify -DiskUnitTests' 
            }
        }
        stage('Maven Build') {
            steps{
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Code Analysis') {
            
            steps {

                 withSonarQubeEnv(credentialsId: 'sonartoken') 
                  sh 'mvn clean package sonar:sonar' 
                  
               } 
            }
           stage('Quality Gate Status') {
            
            steps {
                script{
                  
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonartoken'
                 
            
                }    
              }
           }
            stage('Nexus Artifact Uploader') {
                steps {
                    script{
                        nexusArtifactUploader artifacts: 
                        [
                            [
                                artifactId: 'springboot', 
                                classifier: '', file: 'target/Uber.jar', 
                                type: 'jar'
                            ]
                        ], credentialsId: 'nexuslogin',
                           groupId: 'com.example', 
                           nexusUrl: 'http://35.178.194.39:8081', 
                           nexusVersion: 'nexus3', 
                           protocol: 'http', 
                           repository: 'jenkins-java-app', 
                           version: '1.0.0'
                    }
                }
            }
     }
  }
}     


