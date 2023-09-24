pipeline{
    agent any
    
    stages{
        stage('sonar quality check'){
            agent{
                docker{
                    image 'maven:latest'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarqube') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }        
    }
}