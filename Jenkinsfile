
pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = "${AWS_ACCOUNT_ID}"
        AWS_DEFAULT_REGION="${AWS_DEFAULT_REGION}" // ap-south-1
	    CLUSTER_NAME="${CLUSTER_NAME}" // django-webapp-dev-cluster
	    SERVICE_NAME="${SERVICE_NAME}" // django-webapp-dev-service
	    TASK_DEFINITION_NAME="${TASK_DEFINITION_NAME}" // django-webapp-dev-task-definition
	    DESIRED_COUNT="${DESIRED_COUNT}"
        IMAGE_REPO_NAME="${IMAGE_REPO_NAME}" // django-cicd-with-docker-repo
        IMAGE_TAG="${IAMGE_TAG}" //latest
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
	    registryCredential = "aws-admin-user" //aws-admin-user
    }
   
    stages {

    // // Tests
    // stage('Unit Tests') {
    //   steps{
    //     script {
    //       sh 'npm install'
	//   sh 'npm test -- --watchAll=false'
    //     }
    //   }
    // }
        
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
			docker.withRegistry("https://" + REPOSITORY_URI, "ecr:${AWS_DEFAULT_REGION}:" + registryCredential) {
                    	dockerImage.push()
                	}
         }
        }
      }
      
    // stage('Deploy') {
    //  steps{
    //         withAWS(credentials: registryCredential, region: "${AWS_DEFAULT_REGION}") {
    //             script {
	// 		sh './script.sh'
    //             }
    //         } 
    //     }
    //   }      
      
    }
}
