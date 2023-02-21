# artifact-registry
Artifact Registry is a single place to manage container images and language packages, and is fully integrated with Google Cloud tooling and and runtimes. This makes it simple to integrate it with your CI/CD tooling to set up automated pipelines

# Task 1. Prepare environment

Set up variables<br />
In Cloud Shell, set your project ID and project number. Save them as PROJECT_ID and PROJECT_NUMBER variables:

```
export PROJECT_ID=$(gcloud config get-value project)
export PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format='value(projectNumber)')
export REGION="REGION"
gcloud config set compute/region $REGION
```

Enable Google services<br />
Run the following to enable necessary Google services:

```
gcloud services enable \
  cloudresourcemanager.googleapis.com \
  container.googleapis.com \
  artifactregistry.googleapis.com \
  containerregistry.googleapis.com \
  containerscanning.googleapis.com
```

Get the source code
The source code for this lab is located in the GoogleCloudPlatform org on GitHub.

Clone the source code with the command below, then change into the directory.  

```
git clone https://github.com/GoogleCloudPlatform/cloud-code-samples/
cd ~/cloud-code-samples
```

Provision the infrastructure used in this lab
In this lab you will deploy code to Kubernetes Engine (GKE).

Run the setup script below to prepare this infrastructure:

```
gcloud container clusters create container-dev-cluster --zone="ZONE"
```

# Task 2. Working with container images

Create a Docker Repository on Artifact registry  
Artifact Registry supports managing container images and language packages. Different artifact types require different specifications. For example, the requests for Maven dependencies are different from requests for Node dependencies.  

To support the different API specifications, Artifact Registry needs to know what format you want the API responses to follow. To do this you will create a repository and pass in the --repository-format flag indicating the type of repository desired.  

From Cloud Shell run the following command to create a repository for Docker images:  

```
gcloud artifacts repositories create container-dev-repo --repository-format=docker \
  --location=$REGION \
  --description="Docker repository for Container Dev Workshop"
```


Click Authorize if the Cloud Shell authorization prompt appears.

In the Cloud console, go to Artifact Registry > Repositories and notice your newly created Docker repository named container-dev-repo. If you click on it you can see that it's empty at the moment.

Configure Docker Authentication to Artifact Registry
When connecting to Artifact Registry credentials are required in order to provide access. Rather than set up separate credentials, Docker can be configured to use your gcloud credentials seamlessly.

From Cloud Shell run the following command to configure Docker to use the Google Cloud CLI to authenticate requests to Artifact Registry in the <filled in at lab start> region:

```
gcloud auth configure-docker "Filled in at lab start"-docker.pkg.dev
```

The command will prompt for a confirmation to change the Cloud Shell docker configuration, click ENTER.
  
Explore the sample Application
A sample application is provided in the git repository you cloned.

Change into the java directory and review the application code:

```
cd ~/cloud-code-samples/java/java-hello-world
```
The folder contains an example Java application that renders a simple web page: in addition to various files not relevant for this specific lab, it contains the source code, under the src folder, and a Dockerfile you will use to build a container image locally.

Build the Container Image
Before you can store container images in Artifact Registry you need to create one.

Run the following command to build the container image and tag it properly:

```
docker build -t "REGION"-docker.pkg.dev/"PROJECT_ID"/container-dev-repo/java-hello-world:tag1 .
```

Push the Container Image to Artifact Registry
Run the following command to push the container image to the repository you created:
  
```
docker push "REGION"-docker.pkg.dev/"PROJECT_ID"/container-dev-repo/java-hello-world:tag1
```


Review the image in Artifact Registry
In Artifact Registry > Repositories, click into container-dev-repo and check that the java-hello-world image is there.

Click on the image and note the image tagged tag1. You can see that Vulnerability Scanning is running or already completed and the number of vulnerabilities detected is visible.
  

![](artifact_registry_repositories.png)
  
Click on the number of vulnerabilities and you will see the list of vulnerabilities detected in the image, with the CVE bulletin name and the severity. Click VIEW on each listed vulnerability to get more details:
  
![](cve_bulletin_vulnerability_view.png)
  
  
# Task 3. Integration with Cloud Code
  




































