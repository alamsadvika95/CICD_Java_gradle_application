pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("Sonar Quality Check"){
            agent { 
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh 'java -version'
                        sh './gradlew sonarqube'
                    }
                }
            }
        }
        stage("Docker Build and Docker Push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker-password')]) {
                        sh '''
                            docker build -t 104.198.125.254:8083/springapp:${VERSION} . 
                            docker login -u admin -p $docker-password 104.198.125.254:8083
                            docker push 104.198.125.254:8083/springapp:${VERSION}
                            docker rmi 104.198.125.254:8083/springapp:${VERSION}
                        '''
                    }   
                }
            }
        }
    }
}
