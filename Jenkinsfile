pipeline {
    agent any
    triggers {
        // Poll SCM every 5 minutes
        pollSCM('H/5 * * * *')
    }

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
        stage('Interact with Kubernetes') {
            steps {
                script {
                    sh 'kubectl get pods'
                }
            }
        }
    }
    post {
        success {
            emailext (
                to: 'recipient@example.com',
                subject: "Build Successful: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                body: "The build was successful. Check the console output for details.\n\n${env.BUILD_URL}"
            )
        }
        failure {
            emailext (
                to: 'recipient@example.com',
                subject: "Build Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                body: "The build failed. Check the console output for details.\n\n${env.BUILD_URL}",
                attachLog: true
            )
        }
        unstable {
            emailext (
                to: 'recipient@example.com',
                subject: "Build Unstable: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                body: "The build is unstable. Check the console output for details.\n\n${env.BUILD_URL}",
                attachLog: true
            )
        }
        aborted {
            emailext (
                to: 'recipient@example.com',
                subject: "Build Aborted: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                body: "The build was aborted. Check the console output for details.\n\n${env.BUILD_URL}"
            )
        }
    }
}
