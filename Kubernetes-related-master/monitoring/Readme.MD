===============================================

AWS Opemserch for VPC Flow Logs

Public = https://www.youtube.com/watch?v=trFW03zP7Uw

Private= https://www.youtube.com/watch?v=trI2RzlU-iY 

Blob= https://dheeraj3choudhary.com/stream-aws-cloudwatch-logs-to-amazon-opensearch-service-successor-to-amazon-elasticsearch-service

=================



#### Step 1: Configure EKS Cluster ####
#The command creates an Amazon EKS Kubernetes cluster
eksctl create cluster --region=us-east-1 --name=knote --zones us-east-1a,us-east-1b,us-east-1c,us-east-1d,us-east-1f 

#List the two worker nodes of your cluster 
kubectl get nodes

#CD to path - "F:\RekhuAll\AWS\AWS-EKS-Cluster-Prometheus\knote-js-master\04-05\kube"
#submit your configuration to the new Amazon EKS cluster
kubectl apply -f kube

#Watch the Pods being created:
kubectl get pods --watch

#To access the app, you need the public address of the knote Service.
kubectl get service knote





####Step 2: Install Kubernetes Metrics Server####
#Apply Metrics Server manifests which are available on Metrics Server releases making them installable via url:
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.4.3/components.yaml

#Use the following command to verify that the metrics-server deployment is running the desired number of pods:
kubectl get deployment metrics-server -n kube-system

kubectl get pods -n kube-system

#Confirm Metrics server is active.
kubectl get apiservice v1beta1.metrics.k8s.io -o yaml

#Metrics API can also be accessed by using the kubectl top command.
#To display cluster nodes resource usage – CPU/Memory/Storage you’ll run the command:
kubectl top nodes

#Similar command can be used for pods.
kubectl top pods -A

#You can also access use kubectl get –raw to pull raw resource usage metrics for all nodes in the cluster.
kubectl get --raw "/apis/metrics.k8s.io/v1beta1/nodes"





#### Step 3: Install Helm ####
#The Helm package manager for Kubernetes helps you install and manage applications on your Kubernetes cluster.
#With uisng Chocolaty installer
choco install kubernetes-helm
helm version





#### Step 4: Deploying Prometheus on EKS Kubernetes Cluster ####
#Prometheus can be installed on Kubernetes cluster using Operator or with helm
#First create a monitoring namespace.
kubectl create namespace monitoring

#Prometheus needs a way to persist metrics data for historical reference. We’ll use EBS which is provisioned with gp2 storage class.
kubectl get sc

#Add chart repository:
helm repo add stable https://charts.helm.sh/stable

#Deploy Prometheus using Helm.

helm install prometheus stable/prometheus --namespace monitoring --set alertmanager.persistentVolume.storageClass="gp2",server.persistentVolume.storageClass="gp2"

#Confirm PV and PVC are created.

kubectl get pv -n monitoring

kubectl get pvc -n monitoring





#### Step 5: Access Prometheus on EKS Kubernetes Cluster ####

#After installation query all resources in the monitoring namespace:
kubectl get all -n monitoring

#Get the Prometheus server URL by running these commands in the same shell:
$POD_NAME=$(kubectl get pods --namespace monitoring -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")

#Use Kubernetes port forwarding feature to access Prometheus Server.
kubectl --namespace monitoring port-forward $POD_NAME 9090


=================METOD TWO=========OIDC===AND====AWS====Prometheus===========. 

Video Link https://www.youtube.com/watch?v=2qGDV0N14aY

Email Aler Video= https://www.youtube.com/watch?v=g0gBKtcDOYc

Teams Alert from Grafana= https://gregoiredayet.medium.com/monitoring-and-alerting-on-your-kubernetes-cluster-with-prometheus-and-grafana-55e4b427b22d

Teams Alert Video= https://www.youtube.com/watch?v=U6sUFiBkdgk

Complete Video= https://www.youtube.com/watch?v=losQlALIsYY.     

Blob-1= https://www.eksworkshop.com/intermediate/240_monitoring/

##################### Create AWS EKS clsuster #######################################################################

## Chocolatey links
https://chocolatey.org/install

## Pre-requisite links
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

## Create EKS cluster
eksctl create cluster --name eksampcluster --node-type t2.large --nodes 1 --nodes-min 1 --nodes-max 2 --region us-east-1 --zones=us-east-1a,us-east-1b,us-east-1c

## Get EKS Cluster service
eksctl get cluster --name eksampcluster --region us-east-1

## Update Kubeconfig 
aws eks update-kubeconfig --name eksampcluster

## Get EKS Pod data.
kubectl get pods --all-namespaces

## Delete EKS cluster
eksctl delete cluster --name eksampcluster --region us-east-1

##################################################CREATE AWS EKS BACKUP AND RESTORE #######################################################
## Get IODC Provider data.
  aws eks describe-cluster --name eksampcluster --query "cluster.identity.oidc.issuer" --output text
  
  
1.Creating a new permission policy AWSManagedPrometheusWriteAccessPolicy
  aws iam create-policy --policy-name "AWSManagedPrometheusWriteAccessPolicy" --policy-document file://AWSManagedPrometheusWriteAccessPolicy.json 
  
  
2.Create an IAM role for Kubernetes service account
  aws iam create-role --role-name "EKS-AMP-ServiceAccount-Role" --assume-role-policy-document file://TrustPolicy.json --description "SERVICE ACCOUNT IAM ROLE DESCRIPTION" --query "Role.Arn" --output text
  
  
3.Attach the trust and permission policies to the role
  aws iam attach-role-policy --role-name "EKS-AMP-ServiceAccount-Role" --policy-arn "arn:aws:iam::357171621133:policy/AWSManagedPrometheusWriteAccessPolicy"
  
  
4. EKS cluster hosts an OIDC provider with a public discovery endpoint
  eksctl utils associate-iam-oidc-provider --cluster eksampcluster --approve
  
4.Deploying Prometheus server
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  kubectl create ns prometheus
  helm install prometheus-for-amp prometheus-community/prometheus -n prometheus -f ./amp_ingest_override_values.yaml --set serviceAccounts.server.annotations."eks\.amazonaws\.com/role-arn"="arn:aws:iam::357171621133:role/EKS-AMP-ServiceAccount-Role" --set server.remoteWrite[0].url="https://aps-workspaces.us-east-1.amazonaws.com/workspaces/ws-df62d422-be47-4032-aaea-fad52cf0eab2/api/v1/remote_write" --set server.remoteWrite[0].sigv4.region=us-east-1
  
  
5.Visualizing metrics using Grafana
  helm repo add grafana https://grafana.github.io/helm-charts
  kubectl create ns grafana
  helm install grafana-for-amp grafana/grafana -n grafana
  
  
6.Update your Grafana environment
  helm upgrade --install grafana-for-amp grafana/grafana -n grafana -f ./amp_query_override_values.yaml
  

7.The password is obtained from the Kubernetes secret as follows:
  kubectl get secrets grafana-for-amp -n grafana -o jsonpath='{.data.admin-password}'
  
  
8.Access Grafana by forwarding the port to http://localhost:5001
  kubectl port-forward -n grafana pods/GRAFANA_POD_NAME 5001:3000
#############################################################################################################################################
© 2022 GitHub, Inc.
