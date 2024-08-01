NOTE: I have simplified this pipeline for demo.

**#CICD SETUP**

Deploy application using Jenkins Pipeline to GKE Cluster:

**Requirements:**
**Region**: us-central-1
**Project name**: DevOps-MF
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
5. Now create a pipeline job as name - gke-pipeline --> configure --> copy the script from Jenkisfile and paste in pipeline. then save it.

**#Create a Keyfile Service Account in GCP IAM**
<img width="951" alt="image" src="https://github.com/user-attachments/assets/66f8b1b5-c488-42b0-91d6-8f5736a235ef">

**Pipeline Configuration:**

1. Now create a secret credentials in jenkins with id - gke-service-account-key, upload the download key file to jenkins.

<img width="955" alt="image" src="https://github.com/user-attachments/assets/fba8c730-7693-484f-8c87-30f9be23fb8f">


**Now pipeline is created using jenkins file and build manually to check for errors.**

Please refer the screenshot for build success:
<img width="956" alt="image" src="https://github.com/user-attachments/assets/36ff7a1a-24ec-425a-80bb-99e5a45358e9">

<img width="951" alt="image" src="https://github.com/user-attachments/assets/132091d2-3122-4ed4-b02f-21cfa1f52f0b">

Output:
As you can see, the myapp1 deployment is created and Service- LB also created.
<img width="958" alt="image" src="https://github.com/user-attachments/assets/029e2c36-79a4-475d-8fdd-d981d3cb2b27">

we can access our application using endpoints:

Login to Jenkins server, To check manually, 
$ kubectl get deploy

$ kubectl get pods

$ kubectl get svc

In svc, we can find the external ip to access our application

<img width="958" alt="image" src="https://github.com/user-attachments/assets/8ccbb8bf-5713-4345-95fb-a265b2d1b430">




To check through console,
<img width="950" alt="image" src="https://github.com/user-attachments/assets/8f7eabb4-3027-48de-b970-399ef8a30cac">

<img width="958" alt="image" src="https://github.com/user-attachments/assets/c8777097-1e0d-4cf3-9fdd-5f502e13d5f2">













