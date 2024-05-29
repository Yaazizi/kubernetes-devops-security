pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub')
    }

    stages {
        stage('Build Artifact') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts  'target/*.jar'
            }
        }

        stage('Unit test - JUnit and JaCoCo') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco execPattern: 'target/jacoco.exec'
                }
            }
        }

        stage('Docker Bulid and Push'){
          steps{
            withDockerRegistry([credentialsId:"$DOCKER_CREDENTIALS_USR",url:""]){
              sh 'printenv'
              sh 'docker build -t docker.io/yasserazizi756/numeric-app:""$GIT_COMMIT"" .'
              sh 'docker push -t docker.io/yasserazizi756/numeric-app:""$GIT_COMMIT""'
          }  
        }
      } 
    }
}
