# Jenkins CI/CD Pipeline To Build and Deploy a Hello World app (ITI Final Project)

Jenkins application running as a pod on a GKE cluster (Google Cloud Platform resource) which is used to build and deploy a Hello World python app

This project is based on another repository: https://github.com/shassem/GCPTerraform-Jenkins 

## Illustration & Setup

#### Jenkins Credentials Configurations:
- Create a credential for your Dockerhub account.
- Secret file credentials that contains the VM service account key pair file in order to have access to the cluster.

#### Dockerfile

Created a Dockerfile to dockerize my python app on my local machine.
```bash
    docker build -t shassem/hellopython .
    docker push shassem/hellopython
```
#### Jenkinsfile

Created a Jenkinsfile with continuous integration (CI) and continuous deployment (CD) stages.

CI stage:

- Building the image with a version number equals to the Jenkins build number. 
- Passing the Dockerhub credentials in order to login.
- Pushing the new image to Dockerhub.

BUILD_NUMBER is an environment variable.

CD stage:

- Passing the service account credentials to connect to the cluster
- Replacing the "tag" in the deployment file with the new BUILD_NUMBER (version).
- Deploying the app with kubectl

When the pipeline finishes building, the app will be deployed on the GKE cluster based on this repository:
https://github.com/shassem/GCPTerraform-Jenkins 

### Voila! The application is successfully deployed!🚀 







