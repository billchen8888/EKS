# EKS
We can use eksctl or aws eks to create EKS cluster and the nodegroup

it is good to have kubectl  and aws-iam-authenticaor  in the $PATH on the machine running the eksctl command, as the .kube/config will be copied to this machine for us to run command down the road. (if no kubectl , the eksctl will report “kubectl not found” in one step, but the whole stack is created, it is just the .kube  is not generated.   If only no aws-iam-authenticator, eksclt will print out one error line on screen, but the .kube.config is create.  After the I put aws-iam-authantcator later, I don’t see any function was impacted.

curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.17.9/2020-08-04/bin/linux/<amd64>/aws-iam-authenticator
curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/kubectl
or
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl   #kubectl 1.19.2


https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html#install-eksctl-linux
(curl -LO --silent --location https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz    (eksctl 0.29.0.   supports k8s v1.17, as of 2020-10-14)
Or  curl -LO https://github.com/weaveworks/eksctl/releases/download/0.29.2/eksctl_Linux_amd64.tar.gz  #  file is same as previous one

# 1) unmanaged nodegroup
eksctl create cluster --name eksbyeksctl04  --version 1.17 |
--nodegroup-name myworkernodegrp0401  \
--node-type t3.medium --nodes 4 --nodes-min 1 --nodes-max 4 \
--node-ami auto \
--ssh-access


# 2) Use eksctl –argument cli to create eks cluster and MANAGED nodegroup (default subnet,  in public subnet)

./eksctl create cluster --name eksbyeksctlwithmanagedng02  --version 1.17 --nodegroup-name mymanagedng01 --managed  --node-type t3.medium --nodes 2  --nodes-min 1 --nodes-max 4  --ssh-access
# the --node-ami  is NOT supported when ---managed  is specified.
# this will create its own VPC, public subnets, private subnets.  Security groups etc.

# 3) Use eksctl –argument cli to create eks cluster and MANAGED nodegroup (force managed nodegroup in private subnet)
./eksctl create cluster --name eksbyeksctlwithmanagedng03  --version 1.17 --nodegroup-name mymanagedng01 --managed  --node-type t3.medium --nodes 2  --nodes-min 1 --nodes-max 4  --ssh-access  --node-private-networking


