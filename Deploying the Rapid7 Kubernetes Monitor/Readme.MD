https://qualysapi.qg3.apps.qualys.com

Installation Instructions
Run this in your Kubernetes terminal:
kubectl create secret generic rapid7-monitor --from-literal=monitor-key="us:78c224ed-89da-43f0-88f4-9f95b42d7527" --namespace="default"

Then apply the config file (attached to this email) by running this command:
kubectl apply -f kubernetes-monitor.yml

Kubernetes Monitor
The Rapid7 Kubernetes Monitor is intended to be deployed to Kubernetes clusters and configured to identify instantiated containers. Note that users will need to deploy only one monitor per cluster.

https://hub.docker.com/r/rapid7/kubernetes-monitor

Deploying the Rapid7 Kubernetes Monitor
Integrating your Kubernetes environment with InsightVM can be accomplished by pulling the Rapid7 Kubernetes Monitor from DockerHub, deploy this to each cluster, and performing a few configuration steps. Once configured, data will appear in the Container Security section of InsightVM.

Viewing Kubernetes data
After the monitor is deployed and configured, users will see Kubernetes data in the following areas of the Container Security section of InsightVM:
