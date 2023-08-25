pipeline{
    agent any
    stages{
        stage("Build"){
            steps{
                sh 'mvn clean install -Dskiptests'
            }
        }
        stage("Docker image"){
            steps{
                sh "docker build -t spring-petclinic:v1 ."
                sh "docker tag spring-petclinic:v1 rschatterjee/spring-petclinic:v1"
            }
        }
        stage("Docker registry"){
            steps{
                sh "docker push rschatterjee/spring-petclinic:v1"
            }
        }
        
        stage("Run"){
            steps{
                sh CONTAINER_ID="docker ps -l | grep "spring" | awk '{print $1}'"
                sh docker stop $CONTAINER_ID
                sh "docker run -d -p 1234:8080 spring-petclinic:v1"
            }
        }
        stage("Dowload Artifacts"){
            steps{
       post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', 
        onlyIfSuccessful: true
            }
        }
    }
}
}
}
