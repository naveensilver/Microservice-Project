### Why Microservices?

Microservices is an architectural style where an application is built as a collection of small, independent services. Each service focuses on a specific business function and operates on its own. These services communicate with each other through well-defined APIs, making the system more modular and easier to manage.

### E-Commerce Website Using 10 Microservices on EKS Cluster

#### Project Overview

- **Project Name**: 11 Microservice CI/CD Pipeline
- **Deployment Platform**: AWS EKS (Elastic Kubernetes Service)
- **Application Type**: E-Commerce Website

#### Microservices Implemented

1. **Ad Sense Service**: Manages advertisements and promotions displayed on the website.
2. **Cart Service**: Handles user shopping carts, including adding, removing, and updating items.
3. **Checkout Service**: Manages the checkout process, including order summary and confirmation.
4. **Currency Service**: Provides currency conversion and pricing in different currencies.
5. **Email Service**: Sends emails to users for order confirmations, promotions, and notifications.
6. **Frontend Service**: Manages the user interface and user experience of the website.
7. **External Frontend (for load balancing)**: Distributes incoming traffic across multiple frontend instances to ensure high availability and performance.
8. **Payment Service**: Processes payments and handles transactions securely.
9. **Product Catalogue Service**: Manages product listings, details, and inventory.
10. **Recommendation Service**: Suggests products to users based on their browsing and purchase history.

These microservices can be deployed on an EKS cluster, which provides a managed Kubernetes environment to run containerized applications. Each microservice runs in its own container, and Kubernetes manages the deployment, scaling, and operation of these containers.

#### Reasons for Choosing Microservices

1. **Scalability**: Microservices allow individual services to scale independently based on their specific demand. For instance, the Cart Service can be scaled during high traffic periods without affecting other services.
2. **Resilience**: In a microservices architecture, the failure of one service does not necessarily impact the entire system. Each service can be designed to handle failures gracefully, thereby improving the overall resilience of the application.
3. **Development Speed**: Development teams can work on different services simultaneously without waiting for other teams. This parallel development accelerates the overall development process and allows for quicker releases.
4. **Technology Diversity**: Each microservice can be developed using the most suitable technology stack for its specific needs. For example, the Product Catalogue Service can use a NoSQL database for better performance, while the Checkout Service might use a relational database for transaction consistency.
5. **Isolation and Maintenance**: Microservices encapsulate their own logic, making it easier to understand, develop, test, and maintain. Changes in one service do not directly affect others, reducing the risk and complexity of updates.

#### Project Components and Pipeline

- **CI/CD Pipeline Using Jenkins**:
  - **Jenkinsfiles**: Each microservice has its own Jenkinsfile defining the steps for building, testing, and deploying the service.
  - **Docker**: Microservices are containerized using Docker, allowing for consistent environments from development to production.
  - **AWS EKS**: The microservices are deployed on AWS EKS, a managed Kubernetes service, which orchestrates and manages the lifecycle of the containers.

You can find more details in the [GitHub repository](https://github.com/naveensilver/Microservice-Project.git).

#### Architecture

**Phase 1: Infrastructure Setup**

1. **Create a User in AWS IAM**: Create a new user with any name.
2. **Attach Policies**: Attach the following policies to the newly created user:
   - AmazonEC2FullAccess
   - AmazonEKS_CNI_Policy
   - AmazonEKSClusterPolicy
   - AmazonEKSWorkerNodePolicy
   - AWSCloudFormationFullAccess
   - IAMFullAccess
3. **Create an Inline Policy**: Create an inline policy with the following content and attach it to your user:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "VisualEditor0",
         "Effect": "Allow",
         "Action": "eks:*",
         "Resource": "*"
       }
     ]
   }
   ```
4. **Generate Secret Access Key**: Once the IAM user is created, generate its Secret Access Key and download the `credentials.csv` file.

**Phase 2: Launch Virtual Machine Using AWS EC2**

1. **EC2 Instance Requirements and Setup**:
   - **Instance Type**: `t2.large`
     - vCPUs: 2
     - Memory: 8 GB
     - Network Performance: Moderate
   - **Amazon Machine Image (AMI)**: Ubuntu Server 20.04 LTS (Focal Fossa)
   - **Security Groups**: Security groups act as a virtual firewall for your instance to control inbound and outbound traffic.

2. **SSH into the Server**: After launching your virtual machine, SSH into the server.

**Phase 3: Install AWS CLI, EKSCTL & KUBECTL on VM Server**

1. **AWS CLI**:
   ```sh
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   sudo apt install unzip
   unzip awscliv2.zip
   sudo ./aws/install
   ```

2. **KUBECTL**:
   ```sh
   curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
   chmod +x ./kubectl
   sudo mv ./kubectl /usr/local/bin
   kubectl version --short --client
   ```

3. **EKSCTL**:
   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version
   ```

4. **Save All Scripts**: Save all the scripts in a file, for example, `ctl.sh`, and make it executable using:
   ```sh
   chmod +x ctl.sh
   ```

5. **Run the Script**:
   ```sh
   ./ctl.sh
   ```

#### AWS Configure Steps

To configure AWS CLI, use the following command:
```sh
aws configure
```
This command will prompt you to enter your AWS Access Key ID, Secret Access Key, region, and output format. This setup allows the AWS CLI to interact with your AWS account.

Would you like more details on any specific part of this setup?

### Installing Java, Jenkins, and Docker on Ubuntu

Execute these commands on the Jenkins server:

1. **Install OpenJDK 17 JRE Headless**:
   ```sh
   sudo apt install openjdk-17-jre-headless -y
   ```

2. **Download Jenkins GPG Key**:
   ```sh
   sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
   ```

3. **Add Jenkins Repository to Package Manager Sources**:
   ```sh
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   ```

4. **Update Package Manager Repositories**:
   ```sh
   sudo apt-get update
   ```

5. **Install Jenkins**:
   ```sh
   sudo apt-get install jenkins -y
   ```

Save this script in a file, for example, `install_jenkins.sh`, and make it executable using:
```sh
chmod +x install_jenkins.sh
```

Then, you can run the script using:
```sh
./install_jenkins.sh
```

This script will automate the installation process of OpenJDK 17 JRE Headless and Jenkins.

#### Install Docker

To install Docker, follow these steps:

1. **Update the Package List**:
   ```sh
   sudo apt-get update
   ```

2. **Install Required Packages**:
   ```sh
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common -y
   ```

3. **Add Docker’s Official GPG Key**:
   ```sh
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

4. **Add Docker Repository**:
   ```sh
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

5. **Update the Package List Again**:
   ```sh
   sudo apt-get update
   ```

6. **Install Docker**:
   ```sh
   sudo apt-get install docker-ce -y
   ```

7. **Verify Docker Installation**:
   ```sh
   sudo systemctl status docker
   ```

### Creating an EKS Cluster

To set up an EKS cluster, follow these steps:

1. **Create the EKS Cluster**:
   ```sh
   eksctl create cluster --name=EKS-1 --region=ap-south-1 --zones=ap-south-1a,ap-south-1b --without-nodegroup
   ```
   - **--name**: Specifies the name of the cluster.
   - **--region**: Defines the AWS region where the cluster will be created.
   - **--zones**: Lists the availability zones within the region.
   - **--without-nodegroup**: Creates the cluster without any node groups initially.

2. **Associate IAM OIDC Provider**:
   ```sh
   eksctl utils associate-iam-oidc-provider --region ap-south-1 --cluster EKS-1 --approve
   ```
   - This command associates an OpenID Connect (OIDC) provider with your EKS cluster, which is necessary for IAM roles for service accounts.

3. **Create a Node Group**:
   ```sh
   eksctl create nodegroup --cluster=EKS-1 --region=ap-south-1 --name=node2 --node-type=t3.medium --nodes=3 --nodes-min=2 --nodes-max=4 --node-volume-size=20 --ssh-access --ssh-public-key=DevOps --managed --asg-access --external-dns-access --full-ecr-access --appmesh-access --alb-ingress-access
   ```
   - **--cluster**: Specifies the name of the cluster to which the node group will be added.
   - **--region**: Defines the AWS region where the node group will be created.
   - **--name**: Names the node group.
   - **--node-type**: Specifies the instance type for the nodes.
   - **--nodes**: Sets the initial number of nodes.
   - **--nodes-min**: Sets the minimum number of nodes.
   - **--nodes-max**: Sets the maximum number of nodes.
   - **--node-volume-size**: Defines the size of the EBS volumes attached to each node.
   - **--ssh-access**: Enables SSH access to the nodes.
   - **--ssh-public-key**: Specifies the SSH public key to use for SSH access.
   - **--managed**: Indicates that the node group is managed by EKS.
   - **--asg-access**: Grants access to the Auto Scaling Group.
   - **--external-dns-access**: Grants access to external DNS.
   - **--full-ecr-access**: Grants full access to Amazon ECR.
   - **--appmesh-access**: Grants access to AWS App Mesh.
   - **--alb-ingress-access**: Grants access to the ALB Ingress Controller.

**Note**: Make sure to replace the cluster name, region, zones, and SSH public key with your specific details.





