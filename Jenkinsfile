pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub')
        GIT_COMMIT = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
    }
    stages {
        stage('Build Artifact - Maven') {
            steps {            
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts 'target/*.jar'
            }
        }

        stage('Unit Tests - JUnit and JaCoCo') {
            steps {
                sh "mvn test"
            }
            
        }

        stage('Docker Build and Push') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/']) {
                    sh 'printenv'
                    sh "docker build -t yasserazizi756/numeric-app:$GIT_COMMIT ."
                    sh "docker push yasserazizi756/numeric-app:$GIT_COMMIT"
                }
            }
        }
    }
} //
