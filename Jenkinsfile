pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    
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
        stage('Quality gate status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube'
                }
            }
        } 
        stage('Docker build & docker push to nexus repo'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus', variable: 'nexus_cred')]) {  
                    sh '''
                        docker build -t 44.211.222.177:9090/springapp:${VERSION} .
                        docker login -u admin -p $nexus_cred 44.211.222.177:9090
                        docker push 44.211.222.177:9090/springapp:${VERSION}
                        docker rmi 44.211.222.177:9090/springapp:${VERSION}
                    '''
                    }
                }
            }
        }     
    }
}