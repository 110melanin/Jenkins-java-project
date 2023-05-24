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
                script{
                 withSonarQubeEnv(credentialsId: 'sonartoken'){
                  sh '''mvn clean package sonar:sonar \
                    -Dsonar.projectKey=Jenkins-java-app1 \
                    -Dsonar.host.url=http://3.9.181.151 \
                    -Dsonar.login=6c2df13cff23b3d25b5826d04e796ba99982b4a5'''
                       }

                    }
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


