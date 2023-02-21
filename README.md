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
