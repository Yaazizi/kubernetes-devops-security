pipeline {
    agent any

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
            withDockerRegistry([credentialsId:"docker-hub",url:""]){
              sh 'printenv'
              sh 'docker build -t yasserazizi756/numeric-app:""$GIT_COMMIT"" .'
              sh 'docker push -t yasserazizi756/numeric-app:""$GIT_COMMIT""'
          }  
        }
      } 
    }
}
