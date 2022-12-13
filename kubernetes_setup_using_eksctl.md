# Setup Kubernetes on Amazon EKS

You can follow same procedure in the official  AWS document [Getting started with Amazon EKS – eksctl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)   

#### Pre-requisites: 
  - an EC2 Instance 

#### AWS EKS Setup 
1. Setup kubectl   
   a. Download kubectl version 1.20   ( https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html )
   b. Grant execution permissions to kubectl executable   
   c. Move kubectl onto /usr/local/bin   
   d. Test that your kubectl installation was successful    
   ```sh 
   curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.20.15/2022-10-31/bin/linux/amd64/kubectl
   chmod +x ./kubectl
   mv ./kubectl /usr/local/bin 
   kubectl version --short --client
   ```
2. Setup eksctl   
   a. Download and extract the latest release   
   b. Move the extracted binary to /usr/local/bin   
   c. Test that your eksclt installation was successful   
   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version
   ```
  
3. Create an IAM Role and attache it to EC2 instance    
   `Note: create IAM user with programmatic access if your bootstrap system is outside of AWS`   
   IAM user should have access to   
   a) IAM -> Role -> Create Role -> click on (AWS service Allow AWS services like EC2, Lambda, or others to perform actions in this account.) -> Click on ( EC2) ->     Next.
   b) Give the Permision policy .
     1) IAMFullAccess
     2) AWSCloudFormationFullAccess
     3) AmazonVPCFullAccess
     4) AmazonEC2FullAccess
     5) AdministratorAccess

   c) Next -> Next -> Give the Role Name (   ......... ) -> Create Role 
   Note :- Here , Node Create Successfully.
   d) Go to the Our Ec2 Instance -> Click on the Ec2-instance -> Action -> Security -> Modify IAM ROLE -> Here we need to choose the Give Create Role Name -> Update     IAM Role.
   
4. Create your cluster and nodes 
   ```sh
   eksctl create cluster --name cluster-name  \
   --region region-name \
   --node-type instance-type \
   --nodes-min 2 \
   --nodes-max 2 \ 
   --zones <AZ-1>,<AZ-2>
   
   example:
   eksctl create cluster --name valaxy-cluster \
      --region ap-south-1 \
   --node-type t2.small 
    ```

5. To delete the EKS clsuter 
   ```sh 
   eksctl delete cluster valaxy --region ap-south-1
   ```
   
6. Validate your cluster using by creating by checking nodes and by creating a pod 
   ```sh 
   kubectl get nodes
   kubectl run pod tomcat --image=tomcat 
   ```

