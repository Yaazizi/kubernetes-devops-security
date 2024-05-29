pipeline {
    agent any
    environment {
        DOCKER_USERNAME = 'yasserazizi756'
        DOCKER_PASSWORD = 'Yasser722@722'
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
                withDockerRegistry([credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/']) {
                    script {
                        sh 'printenv'
                        sh "echo '$DOCKER_PASSWORD' | docker login -u $DOCKER_USERNAME --password-stdin https://index.docker.io/v1/"
                        docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                            sh "docker build -t docker.io/yasserazizi756/numeric-app:$GIT_COMMIT ."
                            sh "docker push docker.io/yasserazizi756/numeric-app:$GIT_COMMIT"
                        }
                    }
                }
            }
        }
    }
}
