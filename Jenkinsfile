pipeline{
    agent any
    
    stages{
        stage('sonar quality check'){
            agent{
                docker{
                    image 'maven:latest'
                    args '-u 1000:1000'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarqube') {
                        sh 'useradd -u 1000 -m myuser'
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }        
    }
}