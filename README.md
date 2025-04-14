# Commends to run the application:
## Konfiguracja AWS CLI
```
aws configure
```
## aws cluster creation
```
```
## aws nodes creation
```
aws eks create-nodegroup --cluster-name lsc-cluster --nodegroup-name lsc-ng --node-role arn:aws:iam::363526937044:role/LabRole --subnet subnet-0ac2fdb7d16be0d0d subnet-053c86760f165cd88 --instance-types t3.medium --scaling-config minSize=1,maxSize=2,desiredSize=1 --disk-size 20 --ami-type AL2_x86_64
```
### check cluster status
```
aws eks describe-cluster --region us-east-1 --name lsc-cluster  --query cluster.status
```
## kubctl config update
```
aws eks --region us-east-1 update-kubeconfig --name lsc-cluster
```
## helm installation
```
choco install kubernetes-helm
```
## NFS installation
```
helm repo add nfs-ganesha-server-and-external-provisioner https://kubernetes-sigs.github.io/nfs-ganesha-server-and-external-provisioner/
helm install nfs-server-provisioner nfs-ganesha-server-and-external-provisioner/nfs-server-provisioner -f nfs-values.yaml
```
## config files application
```
kubectl apply -f pvc.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f job.yaml
```
## get http-service url
```
kubectl get service nginx-service
```
# Components chars
![image](https://github.com/user-attachments/assets/8ccd04a7-ba77-4784-8a16-3ba0a15517fd)

Diagram explanation:
•	EKS Cluster: The foundational Kubernetes setup hosted on AWS using Elastic Kubernetes Service.

•	NFS Server & Provisioner: Supplies a shared filesystem for Kubernetes pods through the NFS protocol. The provisioner ensures that PersistentVolumes (PVs) are created automatically based on demand.

•	StorageClass (nfs): A template defining how dynamic volumes should be provisioned using the NFS backend.

•	PersistentVolumeClaim (PVC): A resource used by applications to request specific storage space from the dynamically created NFS volumes.

•	Deployment: Hosts a web server (such as nginx or Apache) with the NFS volume mounted to serve static content.

•	Job: A one-time task that writes a static HTML file (like index.html) into the shared NFS-mounted volume for the web server to serve.

•	Service: Makes the deployed web server accessible, either internally within the cluster or externally via a LoadBalancer.
