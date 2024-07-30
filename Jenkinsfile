pipeline {
    agent any

    environment {
        // Define environment variables
        GCP_PROJECT_ID = 'devops-mf-430911'
        CLUSTER_NAME = 'devops-task-cluster'
        CLUSTER_LOCATION = 'us-central1'
        IMAGE_NAME = 'us-central1-docker.pkg.dev/devops-mf-430911/gke-artifact-repo1/myapp1'
        DEPLOYMENT_NAME = 'myapp1'
		GOOGLE_APPLICATION_CREDENTIALS = credentials('gke-service-account-key')
        KUBECONFIG = '/root/.kube/config'
    }

    stages {
        stage('git pull') {
            steps {
                git 'https://github.com/barveenmj/getting-started.git'
				
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(IMAGE_NAME)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Authenticate with GCR
                        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                        sh 'gcloud auth configure-docker'
                        sh "docker push ${IMAGE_NAME}"
                    }
                }
            }
        

        stage('Deploy to GKE') {
            steps {
                script {
                    // Authenticate with GKE
                    withCredentials([file(credentialsId: 'gke-service-account-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                        sh 'gcloud config set project ${GCP_PROJECT_ID}'
                        sh 'sudo gcloud container clusters get-credentials ${CLUSTER_NAME} --region ${CLUSTER_LOCATION} --project ${GCP_PROJECT_ID}'

                        // Apply Kubernetes deployment configuration
                    
                        sh 'sudo kubectl apply -f deployment.yaml'
						
                        
                    }
                }
            }
        }
    }

    /*post {
        always {
            // Clean up actions (if necessary)
        }
    }*/
}
