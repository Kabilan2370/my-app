# **Blue/Green Deployment for Strapi on AWS**

### What we are developing ?

Backend API: FastAPI

Model: A simple ML model (e.g., sklearn)

Database : postgres:15

Containerization:

Dockerfile (required)

Docker Compose

### 1. Create ECS Cluster

Create an ECS Cluster with:

Launch type: Fargate


### 2. I have created 2 Security Groups

ALB Security Group

Allow inbound traffic from the internet:

HTTP → Port 80

HTTPS → Port 443

Source: 0.0.0.0/0

Allow all outbound traffic.

ECS Service Security Group

Allow inbound traffic only from ALB:

Port: 1337

Source: ALB Security Group

   

### 3. Create Application Load Balancer (ALB)

Create an Application Load Balancer with 2 different target groups aattached with this

1. Bule target group
2. Green target group
   

### 4. Configure ALB Listeners
Listener on Port 80 (HTTP)

Default action → Forward traffic to Blue Target Group

Listener on Port 443

Default action → Forward traffic to Blue Target Group

During deployments, CodeDeploy will automatically shift traffic between Blue ↔ Green target groups.


### 5. Create ECS Task Definition (Placeholder)

Define a Task Definition with:

Create a Task definition with Fargate and default vpc and subnet.

Container:

Image: placeholder image

Container port: 1337

This task definition will be dynamically updated by CI/CD.


### 6. Create ECS Service (Blue/Green Enabled)

Create ECS Service with Fargate

Deployment controller: CODE_DEPLOY

Attach:

ALB attached with Blue and green Target Group

Assign : ECS Security Group

### 7. Create AWS CodeDeploy Application

Create a CodeDeploy Application choose the Compute platform as ECS

### 8. Create CodeDeploy Deployment Group

Configure Deployment Group with:

⚙️ Deployment Settings

Deployment type: Blue/Green

Traffic control: ALB

Deployment strategy: CodeDeployDefault.ECSCanary10Percent5Minutes
