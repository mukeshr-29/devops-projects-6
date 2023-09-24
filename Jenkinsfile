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
                        sh 'mvn -Duser.home=/tmp clean package sonar:sonar'
                    }
                }
            }
        }        
    }
}