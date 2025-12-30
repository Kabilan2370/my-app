## **Amazon ECS (Elastic Container Service)**

### What is ECS (Elastic Container Service) ?
Amazon Elastic Container Service (ECS) is a fully managed AWS service that helps you easily run, stop, and manage Docker containers, acting as an orchestration tool to deploy, scale, and manage containerized apps without the headache of managing the underlying infrastructure yourself, offering Fargate (serverless) or EC2 (self-managed) launch types for flexibility.

### When to Use ECS ?
when you need to run containerized apps on AWS, especially for web apps, microservices, batch jobs, or ML inference, preferring deep AWS integration (IAM, ALB) and simpler management (Fargate) over full Kubernetes complexity, or if you need control over EC2 instances for specific hardware/compliance

### Core ECS Components :
The core are Cluster, Service, Task Definition, and Task, managing containerized apps

**1. Cluster :** A logical grouping of resources ( EC2 instance or Fargate ) where tasks run.

   **1. EC2 instances (EC2 launch type) :** when you need a high degree of control over the underlying infrastructure, require specific instance types, have custom security or networking requirements, or need to optimize costs through specific pricing models like Spot Instances or Reserved Instances
   
   **2. Fargate capacity (serverless) :** Use AWS Fargate with ECS (Elastic Container Service) for serverless container orchestration when you need to focus on code, not servers, ideal for microservices, APIs, batch jobs, and event-driven apps, especially with unpredictable traffic or short-lived tasks.

**2. Task Definition :** A blueprint describing one or more containers, their images, CPU/memory, ports, and environment variables.

**3. Task :** A running instance of a task definition the smallest unit of execution.

**4. Service :** Maintains a desired count of tasks, handles scaling, load balancing and rolling updates for high availability. 

### ECS Deployment Types
Amazon ECS offers three main deployment types via Deployment Controllers

**1. Rolling Update (Default) :** ECS replaces old tasks with new ones incrementally, controlled by minimumHealthyPercent and maximumPercent parameters.

**2. CodeDeploy Blue/Green (CodeDeploy Controller) :** Creates a new "green" environment alongside the old "blue" environment, shifts a small amount of traffic, verifies, then shifts all traffic, allowing for automatic rollbacks.

**3. External (External Controller) :** Gives you full control to use custom scripts, Terraform or other CI/CD tools (like Jenkins, GitLab) to manage the deployment lifecycle.

### Manual Deployment Process Overview
While a production environment typically uses CI/CD services (like AWS CodePipeline, CodeDeploy, or GitHub Actions), a manual deployment through the AWS Console or CLI generally involves these steps:

**1.Prepare the Container Image :**
   - Create a Docker image for your application.

   - Push the image to a container registry like Amazon ECR.

**2. Create the ECS Cluster :**
   - Go to the ECS console and create a new cluster.

   - Choose the Capacity/Launch Type: Select either Fargate (Serverless) or EC2 (if you choose EC2, you will also need to configure the Auto Scaling Group for the cluster instances).

**3. Create a Task Definition :**

   - Define a new Task Definition, choosing the launch type (Fargate or EC2).

   - Specify the Image URI from ECR, CPU, memory, networking, and the IAM role.

**4. Create an ECS Service :**

   - In the cluster, create a new Service.

   - Select the Task Definition you created.

   - Specify the desired number of tasks (containers) to run.

   - Configure the networking (VPC, subnets, and security groups).

   - Optionally, configure a Load Balancer (Application Load Balancer is common) to distribute traffic across your running tasks.

**5. Run/Deploy :**

   - The Service scheduler will launch the desired number of tasks on the chosen capacity (Fargate or EC2 instances).

   - If using a Load Balancer, the tasks will be registered as targets, and traffic will start flowing to your application.







