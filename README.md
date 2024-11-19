# Welcome to my CI/CD ArgoCD Project
## Overview
The Project demonstrates a CI/CD pipeline using Jenkins, Github, Docker, Kubernetes and ArgoCD. the code gets pulled from GitHub by a Jenkins trigger, it creates a Dockerfile with the code [(index.html)](index.html) and uploads it to DockerHub. Jenkins then deploys the [ArgoCD_app.yaml](argocd_app.yaml) file to create the Kubernetes namespace and deploys the [deployment.yaml](./argocd/deployment.yaml) and [service.yaml](./argocd/service.yaml) from [argocd](./argocd/) directory. The cluster then can be monitored and managed by ArgoCD.

![image](https://github.com/user-attachments/assets/818204aa-db98-45b3-a88a-826ac13ecff0)






### Components & Requirements:
- GitHub: Repository management
- Jenkins: Automation of the CI/CD pipeline for building, testing, and deploying the application
- Docker: Creation of the Image file that runs the containerized application
- DockerHub: Stores the Docker Image
- Kubernetes / Minikube / KinD: Orchestration of the deployment
- Kubectl: Kubernetes command line tool to interact with the cluster
- ArgoCD: Continuous Delivery and management of the Kubernetes deployment

### Setup and Deployment
#### - Set the correct environments:
- The repository can be downloaded and the environments in the Jenkins file can be corrected as needed
#### - Build and Push docker image:
-  Make sure to log in to DockerHub:
```
docker login -u <username>
```
#### - ArgoCD setup:
- Access ArgoCD UI:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
 - Can be accessed through https://localhost:8080
 - Log in with the default username: admin
 - To get the password run:
```
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode; echo
```
- ArgoCD deployment configuration:
 - Repository URL: https://github.com/IsaSubhi/ArgoCD_Project.git
 - Revision: HEAD
 - Folder path: argocd
 - Namespace: myappargocd-ns
 - Create the namespace if it doesn't exist = true
 - Automated: selfHeal: true , prune: true

#### - Access the Application:
 - Forward the local port to the service port of the deployment:
```
kubectl -n myappargocd-ns port-forward service/argocd-deployment-service 8081
```

