pipeline{
    agent any
    
    stages{
        stage('sonar quality check'){
            agent{
                docker{
                    image 'maven'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarqube') {
                        sh 'mvn -Dmaven.repo.local=/tmp/.m2/repository clean package sonar:sonar'
                    }
                }
            }
        }        
    }
}