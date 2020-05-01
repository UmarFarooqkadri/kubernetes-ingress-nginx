# kubernetes-ingress-nginx

Folllow below steps to create a Nginx Ingress controller,deploy a app and point your domain to the new service through ingress

Steps for GCP

# Set your project ID in GCP if not set already

gcloud config set project kubernetes-interview-questions

# Create cluster
gcloud container clusters create cluster-1 \
    --zone us-central1-c --num-nodes=3

# Get the credentials
gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project kubernetes-interview-questions

#  Assign admin permissions to your account to enable cluster roles.

ACCOUNT=$(gcloud info --format='value(config.account)')
kubectl create clusterrolebinding owner-cluster-admin-binding \
    --clusterrole cluster-admin \
    --user $ACCOUNT
	


# Create the Nginx controller deployment using kubectl from the official ingress repo
	
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-0.31.1/deploy/static/provider/cloud/deploy.yaml

# Check if the ingress controller pods are setup

kubectl get pods -n ingress-nginx


# Create file

kubectl apply -f https://raw.githubusercontent.com/UmarFarooqkadri/kubernetes-ingress-nginx/master/nginx-ingress.yaml

Note: The annotations under the labels are very important for integrating with the nginx controller deployment.

# Check the created service if it is attached to the external load balancer.

kubectl get svc -n ingress-nginx

Step 1: create a namespace named dev

kubectl create namespace dev

# Create the deployment using kubectl

kubectl create -f https://raw.githubusercontent.com/UmarFarooqkadri/kubernetes-ingress-nginx/master/hello-app.yaml

# Check the deployment status.

kubectl get deployments -n dev

# Create the service using kubectl

kubectl create -f https://raw.githubusercontent.com/UmarFarooqkadri/kubernetes-ingress-nginx/master/hello-app-service.yaml

# Check the service status

kubectl get svc -n dev

# Create the ingress service for the app

kubectl apply -f https://raw.githubusercontent.com/UmarFarooqkadri/kubernetes-ingress-nginx/master/ingress.yaml

Note: Replace test.apps.example.info with your domain name

#Describe created ingress object created to check the configurations.

kubectl describe ingress  -n dev

Below is the original article which has been useful to read and understand about ingress 

https://devopscube.com/setup-ingress-kubernetes-nginx-controller/
	
