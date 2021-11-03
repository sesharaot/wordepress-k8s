##### Configure WordPress application on kubernates and expose using istio

# High level steps:
-----------------
1. Create vpc with specific CIDR range
2. Create EKS cluster 
3. Install istio 
4. Create secret --for mysql password
5. Create pvc for mysql data
6. Deploy MySQL database
7. Create service on mysql to wordpress server communication
8. Create pvc for wordpress data
9. Deploy wordpress
10. Create service on wordpress for istio gateway redirection
11. Create gateway
12. Create virtual service
13. Final check the URL access


# step-1: Create vpc with specific CIDR range

This need to be created in AWS with sutable range.
we may required atleast 2 public and 2 private subnets.

Use the below link to create VPC.
https://docs.aws.amazon.com/directoryservice/latest/admin-guide/gsg_create_vpc.html#create_vpc


# step-2: Create EKS cluster

This need to be created in AWS side, with our above vpc.

Use below document for creating EKS cluster.
https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html

Use below link for adding node group.
https://docs.aws.amazon.com/eks/latest/userguide/create-managed-node-group.html

Authentication EKS cluster use below link FYR.
https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html


# step-3: Install istio 

Go to the Istio release page to download the installation file for your OS, or download and extract the latest release automatically (Linux or macOS):

$ curl -L https://istio.io/downloadIstio | sh -

Move to the Istio package directory. For example, if the package is istio-1.11.4:

$ cd istio-1.11.4

Add the istioctl client to your path (Linux or macOS):

$ export PATH=$PWD/bin:$PATH

For this installation

$ istioctl install

# step-4: Create secret --for mysql password

Run the below command

kubectl create secret generic mysql-pass --from-literal=password=Hello@123

# step-5: Create pvc for mysql data, 6: Deploy MySQL database, 7: Create service for mysql

Copy and paste content in the mysql.yml and run below command.

kubectl create -f mysql.yml

# step-8: Create pvc for wordpress data, 9: Deploy wordpress 10: Create service on wordpress

Copy and paste content in the wordpress.yml and run below command.

kubectl create -f wordpress.yml

# step-11: Create gateway

Copy and paste content in the wordpress-gateway.yml and run below command.

kubectl create -f wordpress-gateway.yml

# step-12: Create virtual service

Copy and paste content in the wordpress-virtualservice.yml and run below command.

kubectl create -f wordpress-virtualservice.yml

# step-13: Final check the URL access

Check services for istio external to get that run below command.

kubectl get svc -n istio-system

Final try in browser and enjoy it.

