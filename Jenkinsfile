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
                 withSonarQubeEnv('sonarserver'){
                  sh '''mvn sonar:sonar \
                     -Dsonar.projectKey=Jenkins-java-app1 \
                     -Dsonar.host.url=http://3.9.181.151 \
                     -Dsonar.login=8ef99e2e38ec5f4a7a1b27ea55a30e99cfb3b554'''
                       }

                    }
                }
            }
           stage('Quality Gate Status') {
            steps{
                script{
                    waitForQualityGate abortPipeline: true

                }
            }
         }
    }    
}

