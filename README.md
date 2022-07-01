
Kubernetes-Auth= https://github.com/keikoproj/aws-auth
and https://github.com/YSureshN/Kubernetes-ingress-ALB-EKS/blob/master/Kubernetes-dashbard


**others of Kubernets**

https://www.replex.io/blog/how-to-install-access-and-add-heapster-metrics-to-the-kubernetes-dashboard

https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

https://github.com/keikoproj/aws-auth.   (AWS Auth Configmap)

kubectl edit -n kube-system configmap/aws-auth

kubectl describe configmap -n kube-system aws-auth

https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html

RBAC, Cluster Role Binding and Service Account

https://github.com/RobinNagpal/kubernetes-tutorials/blob/master/06_tools/007_alb_ingress/01_eks/Makefile

https://github.com/RobinNagpal/kubernetes-tutorials/tree/master/06_tools/007_alb_ingress/01_eks

https://www.youtube.com/watch?v=S8U7A-eGdOs&t=582s

Last Modified : Dec 28.2020
---------------------------------------------------------
# K8s Dashboard with NodePort in 3 simple steps
---------------------------------------------------------
1. Clone IRSOLS K8s-Dashboard repo to deploy it with automation:
`git clone https://github.com/irsols-devops/kubernetes-dashboard.git`
2. Read through the YAML files and apply to your cluster to install & configure dashboard properly:
`kubectl apply -f ./kubernetes-dashboard/`
3. Get the URL (IP:Port) and token to login to dashboard using get-dash.sh

Enjoy your shiny new cluster dashboard ! 


### Manual DIY (Optional): 
Download the official/recommended K8s dashboard yaml file and modify it. Modified version
is included in this repo. If you'd rather do modification yourself , perform following : 
``wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.1.0/aio/deploy/recommended.yaml``
change the clusterIp to Nodeport as changed in the "kubernetes-dashboard-deployment.yml" file

### **Details of K8s dashboard manual steps with NodePort**

1. Create the deployment : kubectl apply -f kubernetes-dashboard-deployment.yml
2. Create service account admin role :  kubectl apply -f service-account-admin.yml
3. Perform cluster role binding : kubectl apply -f rbac-clusterRoleBinding-admin.yml
4. Ensure K8s deployment and service are up and running :
  a. kubectl get deployments -n kubernetes-dashboard
  b. If this is a multi-node cluster then figure out which node the dashboard pods are running on :
        kubectl get pods --namespace=kubernetes-dashboard -o wide
  c. Double check that there are no issues in the services and its not in CrashLoopBack state:
5. Get the NodePort Dashboard is exposed on using following :
    kubectl get services --namespace=kubernetes-dashboard -o wide
6. Get service token and store in a local file using :
   kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')

7. Open up a browser that allows self-signed certificate using webpages ( e.g. Firefox ) and use the node  IP:Nodeport combination to access the dashboard . e.g. https://<node4>:<node_port> will be https://10.10.0.4:32005

8. In the login window use the token from previous step

  
  =====
  
  
 How to add a liveness and readiness probe? From POM.XML through Maven Plugin

  https://www.eclipse.org/jkube/docs/kubernetes-maven-plugin
  
  
kubernetes-maven-plugin automatically adds Kubernetes liveness and readiness probes in generated Kubernetes manifests in presence of Spring Boot Actuator dependency.

To add actuator to your project, add the following dependency:

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
===================

==========Devops-Related-all-Services-Detailed-Blobs========== 

https://jhooq.com/terragrunt-guide/          (Terragrunt)


https://jhooq.com/categories/terraform/      (Terraform)

https://jhooq.com/categories/kubernetes/       (Kubernetes)

https://jhooq.com/categories/docker/        (Docker)

https://jhooq.com/categories/helm-chart/      (Helm-Chart)

https://jhooq.com/categories/blogging/         (Blogging)

https://jhooq.com/categories/spring-boot/      (Spring-Boot)

https://jhooq.com/categories/quarkus/          (Quarkus)

https://jhooq.com/categories/ssl/           (SSl)

https://jhooq.com/categories/aws/        (AWS)

https://jhooq.com/categories/github/      (Github)

https://jhooq.com/categories/kubespray/    (Kubespray)

https://jhooq.com/categories/prometheus-grafana/     (Prometheus-Grafana)

https://jhooq.com/categories/vagrant/        (Vagrant)

https://jhooq.com/categories/nginx/         (Nginx-in-Kubernetes)
