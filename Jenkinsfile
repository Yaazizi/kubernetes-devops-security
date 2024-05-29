pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub')
        GIT_COMMIT = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
    }
    stages {
        stage('Build Artifact') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts 'target/*.jar'
            }
        }

        stage('Unit Test - JUnit and JaCoCo') {
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

        stage('Docker Build and Push') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub', url: '']) {
                    script {
                        sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
                        sh 'docker build -t docker.io/yasserazizi756/numeric-app:$GIT_COMMIT .'
                        sh 'docker push docker.io/yasserazizi756/numeric-app:$GIT_COMMIT'
                    }
                }
            }
        }
    }
}
