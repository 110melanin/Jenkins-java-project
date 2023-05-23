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
                  sh '''mvn sonar:sonar \
                     -Dsonar.projectKey=Jenkins-java-app2 \
                     -Dsonar.host.url=http://13.41.224.255 \
                     -Dsonar.login=02bf33affaf3fb30772dc319f90b0289aa3d20b7'''

                    }
                }
            }
           stage('Quality Gate Status') {
            steps{
                script{
                    waitForQualityGate abortPipeline: true, credentialsId: 'sonar-api'

                }
            }
         }
    }    
}

