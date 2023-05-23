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
                scripts{
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                    sh 'mvn clean package sonar:sonar'   
                    }
                }
            }
        }
           stage('Quality Gate Status') {
            steps{
                scripts{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }
        }
         
    }
}