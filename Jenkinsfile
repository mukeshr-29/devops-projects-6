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
                    withCredentials([string(credentialsId: 'nexus-pass', variable: 'nexus_creds')]) {
                    sh '''
                        docker build -t 34.227.142.102:8083/springapp:${VERSION} .
                        docker login -u admin -p mukesh123 34.227.142.102:8083
                        docker push 34.227.142.102:8083/springapp:${VERSION}
                        docker rmi 34.227.142.102:8083/springapp:${VERSION}
                    '''
                    }
                }
            }
        }     
    }
}