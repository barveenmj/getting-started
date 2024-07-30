**#CICD SETUP**

Deploy application using Jenkins Pipeline to GKE Cluster:

**Requirements:**
**Region**: us-central-1
**Jenkins Server** - E2 medium, OS type - Centos 9 stream, Data - 30 gb
<img width="958" alt="image" src="https://github.com/user-attachments/assets/93a3d549-b31a-4373-9655-935f41b9fbfc">

**GKE cluster** - 3 nodes
<img width="954" alt="image" src="https://github.com/user-attachments/assets/cef7af35-cb27-491c-b882-03623039df46">

**GKE Artifact registry Setup:**
# Create Google Artifact Registry 
Name: gke-artifact-repo1
Format: Docker
Region: us-central-1
Encryption: Google-managed encryption key
Click on Create

STEPS INVOLVED:
**Jenkins Server Setup:**
1. Install Git, Maven, Docker, Jenkins, Google SDK, Kubectl cli in the jenkins server.
2. Login gcp account using gcloud-auth-login
3. After Jenkins installation, Setup and Login Jenkins server using this url **http://35.232.123.159:8080/**
4. Install plugins like ssh, scp, docker plugin, kubernetes, google kubenetes engine plugins, gcloud sdk, Docker Pipeline in manage jenkins. After installation ,restart it.
5. Now create a pipeline job as name - gke-pipeline --> configure --> copy the script from Jenkisfile adn paste in pipeline. then save it.

**#Create a Keyfile Service Account in GCP IAM**
<img width="951" alt="image" src="https://github.com/user-attachments/assets/66f8b1b5-c488-42b0-91d6-8f5736a235ef">

**Pipeline Configuration:**

1. Now create a secret credentials in jenkins with id - gke-service-account-key, upload the download key file to jenkins.

<img width="955" alt="image" src="https://github.com/user-attachments/assets/fba8c730-7693-484f-8c87-30f9be23fb8f">

**Now pipeline is created using jenkins file and build manually to check for errors.**





