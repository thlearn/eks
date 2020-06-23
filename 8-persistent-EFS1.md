# click through AWS console to create EFS
login to AWS console and search for service _EFS_   
click through wizard , use our  VPC and all 3 AZs
*specify the security group of your EC2-worker-nodes, to be applied to EFS as well*

# add amazon-efs-utils
install the package *amazon-efs-utils* on all worker nodes


```
ssh -i <<eks-key.pem>> ec2-user@<<ec2-workernode>> "sudo yum install -y amazon-efs-utils"
```

# create namespace
```
kubectl create namespace ns-eks-efs
```

# create storage
The steps to execute are:  

1. add RBAC to access EFS
2. create EFS provisioner  
replace your *EFS-ID* and *EFS-DNS name* in file _create-efs-provisioner.yaml_
3. create PVCs

The corresponding commands are:  
```
kubectl apply -f create-rbac.yaml --namespace=ns-eks-efs
kubectl apply -f create-efs-provisioner.yaml --namespace=ns-eks-efs
kubectl apply -f create-storage.yaml --namespace=ns-eks-efs
```

# deploy mysql
## create secret which stores mysql pw, to be injected as env var into container
```
kubectl create secret generic mysql-pass --from-literal=password=eks-mysql-pw --namespace=ns-eks-efs
```
check:
```
kubectl get secrets --namespace=ns-eks-efs
```

## launch mysql deployment
```
kubectl apply -f deploy-mysql.yaml --namespace=ns-eks-efs
```
## checks
* persistent volumes
* persistent volume claims
* pods
```
kubectl get pv --namespace=ns-eks-efs
kubectl get pvc --namespace=ns-eks-efs
kubectl get pods -o wide --namespace=ns-eks-efs
```

# deploy wordpress
```
kubectl apply -f deploy-wordpress.yaml --namespace=ns-eks-efs
```
get URL of the app:
```
kubectl describe service wordpress --namespace=ns-eks-efs | grep Ingress
```
or goto AWS console => EC2

```
