pipeline {
  
  agent any

  stages {

   stage('Build Artifact - Maven') {
    steps {            
            sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
     }

  stage('Unit Tests - JUnit and JaCoCo') {
      steps {
       sh "mvn test"
      }
   }

 //    stage('Mutation Tests - PIT') {
 //      steps {
 //        sh "mvn org.pitest:pitest-maven:mutationCoverage"
 //      }
 //    }

 //    stage('SonarQube - SAST') {
 //      steps {
 //        withSonarQubeEnv('SonarQube') {
 //          sh "mvn sonar:sonar \
	// 	              -Dsonar.projectKey=numeric-application \
	// 	              -Dsonar.host.url=http://devsecops-demo.eastus.cloudapp.azure.com:9000"
 //        }
 //        timeout(time: 2, unit: 'MINUTES') {
 //          script {
 //            waitForQualityGate abortPipeline: true
 //          }
 //        }
 //      }
 //    }

	// stage('Vulnerability Scan - Docker') {
 //      steps {
 //        parallel(
 //        	"Dependency Scan": {
 //        		sh "mvn dependency-check:check"
	// 		},
	// 		"Trivy Scan":{
	// 			sh "bash trivy-docker-image-scan.sh"
	// 		},
	// 		"OPA Conftest":{
	// 			sh 'docker run --rm -v $(pwd):/project openpolicyagent/conftest test --policy opa-docker-security.rego Dockerfile'
	// 		}   	
 //      	)
 //      }
 //    }
    

    stage('Docker Build and Push') {
      steps {
     withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
      sh 'printenv'
       sh 'sudo docker build -t yasserazizi756/numeric-app:""$GIT_COMMIT"" .'
       sh 'docker push yasserazizi756/numeric-app:""$GIT_COMMIT""'
        }
      }
   }
