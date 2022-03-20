# Project-4-Monolith-Application

### Break a Monolith Application into Microservices with Amazon Elastic Container Service, Docker, and Amazon 

In this project, I will deploy a monolithic node.js application to a Docker container, then decouple the application into microservices without any downtime. The node.js application hosts a simple message board with threads and messages between users.

## Why this Matters

Traditional monolithic architectures are hard to scale. As an application's code base grows, it becomes complex to update and maintain. Introducing new features, languages, frameworks, and technologies becomes very hard, limiting innovation and new ideas. Within a microservices architecture, each application component runs as its own service and communicates with other services via a well-defined API. Microservices are built around business capabilities, and each service performs a single function. Microservices can be written using different frameworks and programming languages, and you can deploy them independently, as a single service, or as a group of services.

![monolith_1-monolith-microservices 70b547e30e30b013051d58a93a6e35e77408a2a8](https://user-images.githubusercontent.com/91766546/159107066-6540d69a-b1d9-4f93-8b8a-9c1ba8235a85.png)

During this project, I will demonstrate how to run a simple monolithic application in a Docker container, deploy the same application as microservices, then switch traffic to the microservices without any downtime.

### Monolithic Architecture

As the picture above indicates, the entire node.js application is run in a container as a single service and each container has the same features as all other containers. If one application feature experiences a spike in demand, the entire architecture must be scaled.

### Microservices Architecture

Each feature of the node.js application in the microservices architecture, runs as a separate service within its own container. The services can scale and be updated independently of the others.

This project is made up of five modules.

Module 1: Containerizing the Monolith

Module 2: Deploying the Monolith

Module 3: Breaking the Monolith

Module 4: Deploying Microservices

Module 5: Clean Up

Let's begin.

## Module 1 - Containerizing the Monolith

In this module, I will build the container image for the monolithic node.js application and push it to Amazon Elastic Container Registry. This diagram below demonstrates how the process will go.

![monolith_3-Image-Deployment-to-Amazon-ECR ef4f8b89baccbd37380998a8d896126df5ed8a3b](https://user-images.githubusercontent.com/91766546/159107196-7a9fb913-cbb9-4bd3-834f-d24baaafd89f.png)

Let's start by defining what a container means.

### What Is a Container?

Containers allow you to easily package an application's code, configurations, and dependencies into easy to use building blocks that deliver environmental consistency, operational efficiency, developer productivity, and version control. Containers can help ensure that applications deploy quickly, reliably, and consistently regardless of deployment environment.

## Why Use Containers?

**Speed:**  Launching a container with a new release of code can be done without significant deployment overhead. Operational speed is improved, because code built in a container on a developer’s local machine can be easily moved to a test server by simply moving the container. At build time, this container can be linked to other containers required to run the application stack.

**Dependency Control & Improved Pipeline:**  A Docker container image is a point in time capture of an application's code and dependencies. This allows an engineering organization to create a standard pipeline for the application life cycle. 

**Density & Resource Efficiency:**  Containers facilitate enhanced resource efficiency by allowing multiple heterogeneous processes to run on a single system. Resource efficiency is a natural result of the isolation and allocation techniques that containers use. Containers can be restricted to consume certain amounts of a host's CPU and memory. 

**Flexibility:**  The flexibility of Docker containers is based on their portability, ease of deployment, and small size. In contrast to the installation and configuration required on a VM, packaging services inside of containers allows them to be easily moved between hosts, isolated from failure of other adjacent services, and protected from errant patches or software upgrades on the host system.

For the first part of this project, you will build the Docker container image for your monolithic node.js application and push it to Amazon Elastic Container Registry (Amazon ECR). 

In the next few steps, you are going to be using **Docker**, **Github**, **Amazon Elastic Container Service (Amazon ECS)**, and **Amazon ECR** to deploy code into containers. To complete these steps, ensure you have the following tools.

Prerequisites
1- Have an Aws account.

Create free AWS account. Once you have created your AWS account, navigate to the login page and type in your credentials.

![step1](https://user-images.githubusercontent.com/91766546/155834553-450930ad-5f7e-4bdd-943d-f4db1b92bfba.png)

After signing-in to your AWS account, navigate to the top-right corner of your screen and select your preferred region. This should be the closest region to your physical location.

![step2](https://user-images.githubusercontent.com/91766546/155834557-142107f5-955e-4d04-a516-51bf2392018f.png)

2- Install Docker.

Since I had installed Docker on my terminal from my previous project. I use the following command to check its version:

![15](https://user-images.githubusercontent.com/91766546/159130602-27615ae4-0c28-4d43-99bd-392fec10dcf3.png)

![1111](https://user-images.githubusercontent.com/91766546/159130719-c8a6df71-0d97-460d-a03d-eedaf1c365b6.png)

3- Install the AWS CLI.

Since I had installed the AWS CLI version 2 on my terminal from my previous project. I use the following command to check its version:

![Screenshot from 2022-03-14 21-56-38](https://user-images.githubusercontent.com/91766546/158956922-d94838d6-3143-4dd5-a6a8-2072f6031ec3.png)

4- Have a text editor.

If you don't already have a text editor for coding, install one to your local environment. Atom is a simple, open-source text editor from GitHub that is popular with developers. But, I will be using Visual Studio code (VScode) as text editor for this project.

Ok, now that all our prerequisites have been satisfied, let's get on with the project by downloading the code from GitHub.

Step 1.

Navigate to https://github.com/awslabs/amazon-ecs-nodejs-microservices and select Clone or Download to download the GitHub repository to your local environment.

![g](https://user-images.githubusercontent.com/91766546/159130877-80d8d590-2572-4a7b-80ad-0f138ec1b2e0.png)

![g](https://user-images.githubusercontent.com/91766546/159130919-e7623afb-2952-430a-9adf-1189b07d7865.png)

![17](https://user-images.githubusercontent.com/91766546/159130953-af7dc0ef-7bce-4420-9264-eac5e0dd7793.png)

![ggg](https://user-images.githubusercontent.com/91766546/159131058-892910b1-f063-4834-96c0-66b3fad4337c.png)

Step 2.

Login on AWS account with the IAM User credentials which was provided from the excel sheet when creating the USER. Then navigate to the Amazon ECR console.

![ecr](https://user-images.githubusercontent.com/91766546/159131226-9461a2f0-8515-4fba-9a32-ebc0904f3056.png)

On the Repositories page, select Create Repository and enter the following name your repository: api Note: Under Tag immutability, leave the default settings. Select Create repository.

![p11](https://user-images.githubusercontent.com/91766546/159131170-f91d96f5-efb3-46f2-9f3f-e8d421c8fbb0.png)

![ecr-repo-name1](https://user-images.githubusercontent.com/91766546/159131460-67634e39-bbf7-49cf-bd39-22106f6a9a09.png)

![p2](https://user-images.githubusercontent.com/91766546/159131478-8587125e-5e18-41c9-a7e7-28ec56845b67.png)

Step 3.

Use the terminal to authenticate Docker log in:

*******docker login -u AWS -p $(aws ecr get-login-password --region the-region-you-are-in) xxxxxxxxx.dkr.ecr.the-region-you-are-in.amazonaws.com*******

If needed, configure your credentials.If the authentication was successful, you will receive the confirmation message: Login Succeeded.

![e](https://user-images.githubusercontent.com/91766546/159131972-f777f69f-efbe-4b37-83c0-f04cb1a70747.png)

To build the image, run the following command in the terminal: docker build -t api .

![ee](https://user-images.githubusercontent.com/91766546/159131977-a44ea95d-16d1-4f1f-ba3f-63e433ad2f00.png)

After the build completes, tag the image so you can push it to the repository: 

![eee](https://user-images.githubusercontent.com/91766546/159131982-8e51e8f5-d11f-4068-8f6b-da67e0fd584a.png)

![p2](https://user-images.githubusercontent.com/91766546/159132265-0fe96bc8-49e3-4888-be77-decd13b71407.png)

Push the image to Amazon ECR by running the folowing command.

![eeee](https://user-images.githubusercontent.com/91766546/159131993-4cdc5c49-ce49-420f-93d8-44a70d2d005e.png)

**Note** - replace the [account-ID] and [region] placeholders with your specific information.

If you navigate to your Amazon ECR repository, you should see your image tagged v1.

![p3](https://user-images.githubusercontent.com/91766546/159132293-0d0201df-9741-4281-ba0f-54df3e1f2649.png)

## Module Two - Deploying the Monolith

In this module, I will use Amazon Elastic Container Service (Amazon ECS) to instantiate a managed cluster of EC2 compute instances and deploy your image as a container running on the cluster. 

### Architecture Overview

![pic1](https://user-images.githubusercontent.com/91766546/159132524-c81bf603-4b6b-4723-95bb-abea790f3374.png)

**a. Client:** The client makes a request over port 80 to the load balancer.

**b. Load Balancer:** The load balancer distributes requests across all available ports.

**c. Target Groups:** Instances are registered in the application's target group.

**d. Container Ports:** Each container runs a single application process which binds the node.js cluster parent to port 80 within its namespace.

**e. Containerized node.js Monolith:** The node.js cluster parent is responsible for distributing traffic to the workers within the monolithic application. This architecture is containerized, but still monolithic because each container has all the same features of the rest of the containers.

## What is Amazon Elastic Container Service?

Amazon Elastic Container Service (Amazon ECS) is a highly scalable, high performance container management service that supports Docker containers and allows you to easily run applications on a managed cluster of Amazon EC2 instances. With simple API calls, you can launch and stop Docker-enabled applications, query the complete state of your cluster, and access many familiar features like security groups, Elastic Load Balancing, EBS volumes, and IAM roles.

You can use Amazon ECS to schedule the placement of containers across your cluster based on your resource needs and availability requirements. You can also integrate your own scheduler or third-party schedulers to meet business or application specific requirements.

There is no additional charge for Amazon ECS. You pay for the AWS resources (for example, EC2 instances or EBS volumes) you create to store and run your application.

**Services Used:**

- [Amazon Elastic Container Service](https://aws.amazon.com/ecs/)
- [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/)
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
- [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/)

## Implementation Instructions

Follow the step-by-step instructions below to deploy the node.js application using Amazon ECS. Select each step number to expand the section.

Create an Amazon ECS cluster deployed behind an Application Load Balancer.

1. Navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home).

![Screenshot from 2022-03-11 22-04-57](https://user-images.githubusercontent.com/91766546/159133229-00bea052-cb8e-4062-a2db-e4b087f516d9.png)

![Screenshot from 2022-03-11 22-05-43](https://user-images.githubusercontent.com/91766546/159133299-33a7588f-60c0-47a6-a9f0-e33b73d89025.png)

Select Upload a template file and choose the ecs.yml file from the GitHub project at amazon-ecs-nodejs-microservice/2-containerized/infrastructure/ecs.yml then select Next.

![p6](https://user-images.githubusercontent.com/91766546/159133401-77732836-b4cc-41d8-977c-e973b8025633.png)

![f](https://user-images.githubusercontent.com/91766546/159133364-0861e932-2ee6-4fa2-b28d-1f05452f75a9.png)

For the stack name, enter BreakTheMonolith-Demo. Verify that the other parameters have the following values:
              1. Desired Capacity = *2*
              2. InstanceType = *t2.micro*
              3. MaxSize = *2*

![ff](https://user-images.githubusercontent.com/91766546/159133486-05bc991e-abdb-4b0c-998c-48ac4b2f3795.png)

![fff](https://user-images.githubusercontent.com/91766546/159133497-bf57801b-c230-4293-b354-49d5d0e20196.png)

On the Configure stack options page, keep the default options and scroll down and select Next.

![ffff](https://user-images.githubusercontent.com/91766546/159133505-21339514-0d48-4543-8de3-0da50f24fe59.png)

![fffff](https://user-images.githubusercontent.com/91766546/159133514-b6ca8688-cd29-41e6-b210-b79f3528a5f5.png)

On the Review BreakTheMonolith-Demo page scroll to the bottom of the page, acknowledge the Capabilities statement by selecting the checkbox, and select Create stack.

![Screenshot from 2022-03-11 22-10-36](https://user-images.githubusercontent.com/91766546/159133617-e33eaf01-80a5-4db2-a815-0f45be901e6d.png)

![Screenshot from 2022-03-11 22-10-48](https://user-images.githubusercontent.com/91766546/159133652-b31008af-c04f-4e6f-b2bd-d4eb2bc8668b.png)

You will see your stack with the status CREATE_IN_PROGRESS. You can select the refresh button at the top right of the screen to check on the progress. This process typically takes under 5 minutes.

![p8](https://user-images.githubusercontent.com/91766546/159133547-09b78824-0ac2-480d-9cab-3fedbc0244d2.png)


**Step 2. Check your Cluster is Running**

Navigate to the [Amazon ECS console](https://console.aws.amazon.com/ecs/home?).Your cluster should appear in the list.

![Screenshot from 2022-03-11 22-16-25](https://user-images.githubusercontent.com/91766546/159139086-fb246584-f1fa-4829-bc8b-350cb3cf1801.png)

Select the cluster BreakTheMonolith-Demo, then select the Tasks tab to verify that there are no tasks running.

![h](https://user-images.githubusercontent.com/91766546/159139182-e6e13d2f-0f83-4645-a184-e60f777db692.png)

Select the ECS Instances tab to verify there are two Amazon EC2 instances created by the AWS CloudFormation template.

![hh](https://user-images.githubusercontent.com/91766546/159139220-f48b0543-dcd0-4706-a258-1962fda91d7a.png)

- From the Amazon ECS left navigation menu, select "Task Definitions". Then, select **Create new Task Definition**.

![hhh](https://user-images.githubusercontent.com/91766546/159139318-6845f94d-eeae-46f3-87fe-842c15880ce2.png)

On the "Select launch type compatibility" page, select the "EC2" option then select "Next step".

![hhhh](https://user-images.githubusercontent.com/91766546/159139493-a36a4add-29bf-4f50-bc64-d15d998c9fe0.png)

On the "Configure task and container definitions" page, in the "Task Definition Name" field, enter *api*.
  
![hhhhh](https://user-images.githubusercontent.com/91766546/159139981-a33db207-f911-4aa8-a328-2559e7913157.png)

 Scroll down to "Container Definitions" and select "Add container".

![hhhhhh](https://user-images.githubusercontent.com/91766546/159140017-5eb0e16e-bc66-4b27-8434-520ef24d203b.png)

- In the **Add container** window:
        - In the **Container name** field, enter *api*. In the **Image** field, enter *[account-ID].dkr.ecr.[region].amazonaws.com/api:v1*Replace *[account-ID]* and *[region]* with your specific information. Ensure the tag matches the value you used in Module 1 to tag and push the image. This is the URL of your ECR repository image that was created in the previous module. In the **Memory Limits** field, verify **Hard limit** is selected and enter *256* as the value. Next, under **Port mappings**, Host port = *0* and Container port = *3000*.      
        - 
![i](https://user-images.githubusercontent.com/91766546/159140700-43baf2f6-6b55-4f71-b306-6ec154dfe339.png)   

Scroll to **ENVIRONMENT**, CPU units = *256*. Select **Add**.You will return to the **Configure task and container definitions** page.

![ii](https://user-images.githubusercontent.com/91766546/159140764-e1b9556b-1311-43e7-a46f-7dd83dbf91c2.png)

Scroll to the bottom of the page and select **Create**.

![iii](https://user-images.githubusercontent.com/91766546/159140794-65bbda10-4fa9-45e9-846d-c39b58b65463.png)

![iiii](https://user-images.githubusercontent.com/91766546/159140798-de7f68a2-698b-4242-b0c0-a3762826f473.png)

Your Task Definition is listed in the console.

![k](https://user-images.githubusercontent.com/91766546/159140984-37b1bc13-c540-4050-8b6d-4bcfefe4b91c.png)

![kk](https://user-images.githubusercontent.com/91766546/159140989-542b8773-1e91-49d1-be25-2cbb2a22f525.png)

The [Application Load Balancer (ALB)](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) lets your service accept incoming traffic. The ALB automatically routes traffic to container instances running on your cluster using them as a [target group](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html).

**Check your VPC Name:** If this is not your first time using this AWS account, you may have multiple VPCs. It is important to configure your Target Group with the correct VPC.

- Navigate to the [Load Balancer section of the EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Locate the Load Balancer named **demo**.
- Select the checkbox next to **demo** to see the Load Balancer details.
- In the **Description** tab, locate the **VPC** attribute (in this format: vpc-xxxxxxxxxxxxxxxxx). **Note:** You will need the VPC attribute in the next step when you configure the ALB target group.

![j](https://user-images.githubusercontent.com/91766546/159141095-4dc8f2e0-1c49-4715-9a6f-9c8b2b33630b.png)

Configure the ALB Target Group

Navigate to the Target Group section of the EC2 Console. Select **Create target group**.

![jj](https://user-images.githubusercontent.com/91766546/159141176-c625207b-4f20-4084-8d6d-8fb0c51b0442.png)

Configure the following Target Group parameters (for the parameters not listed below, keep the default values):
    - For the **Target group name**, enter *api*.

![jjj](https://user-images.githubusercontent.com/91766546/159141243-39b61a1b-261c-45f4-a52e-4767f343042d.png)

    - For the **Protocol**, select **HTTP**.
    - For the **Port**, enter *80*.
    - For the VPC, select the value that matches the one from the Load Balancer description.
    
![jjjj](https://user-images.githubusercontent.com/91766546/159141265-efef2d22-991f-454a-becc-16cc7a93f1ad.png)

- Access the **Advanced health check settings** and edit the following parameters as needed:
    - For **Healthy threshold**, enter *2*.
    - For **Unhealthy threshold**, enter *2*.
    - For **Timeout**, enter *5*.
    - For **Interval**, enter *6*.
- Select **Create**.

![jjjjj](https://user-images.githubusercontent.com/91766546/159141312-88236455-eec3-44f9-a1b5-9303464d8603.png)

![jjjjjj](https://user-images.githubusercontent.com/91766546/159141333-c9ae5dbd-e380-4f02-885d-3f9e61f3d19a.png)

![jjjjjjj](https://user-images.githubusercontent.com/91766546/159141347-d69ffc54-28f2-48bb-b057-77bcfaad5a9a.png)


The ALB listener checks for incoming connection requests to your ALB.

Add a Listener to the ALB

Navigate to the Load Balancer section of the EC2 Console.

- Select the **Listeners** tab.
- Select **Add listener** and edit the following parameters as needed:
    - For **Protocol:port**, select **HTTP** and enter *80*.
    - For **Default action(s)**, select **Forward to** and in the **Target group** field, enter *api*.
- Select **Save**.

![ll](https://user-images.githubusercontent.com/91766546/159141391-8c7b4062-8926-4833-9e11-4aee32361d7e.png)


![l](https://user-images.githubusercontent.com/91766546/159141370-4e380d9e-fb6f-49bd-92d0-8da037d39215.png)


![lll](https://user-images.githubusercontent.com/91766546/159161691-9e01b718-d198-4abb-9a99-542778850f90.png)

Deploy the monolith as a service into the cluster, follow these steps.

Navigate to the Amazon ECS console and select Clusters from the left menu bar.
![llll](https://user-images.githubusercontent.com/91766546/159161995-ce51ae08-d643-46e0-8601-2ff14fff9fcc.png)

- Select the cluster **BreakTheMonolith-Demo**, select the **Services** tab then select **Create**.
- On the **Configure service** page, edit the following parameters (and keep the default values for parameters not listed below):
    - For the **Launch type**, select **EC2**.
    - For the **Service name**, enter *api*.
    - For the **Number of tasks**, enter *1*.
    - Select **Next step**.

![lllll](https://user-images.githubusercontent.com/91766546/159162087-ff57501b-920b-4dda-98f0-b2d5c89d1eae.png)


- On the **Configure network** page, **Load balancing** section, select **Application Load Balancer**.Additional parameters will apear: **Service IAM role** and **Load balancer name**.
    - For the **Service IAM role**, select BreakTheMonolith-Demo-ECSServiceRole.
    - For the **Load balancer name**, verify that **demo** is selected.

![llllll](https://user-images.githubusercontent.com/91766546/159162123-72c0d1ff-8909-44c1-9bdf-ef3328beb0dd.png)

![lllllll](https://user-images.githubusercontent.com/91766546/159162172-12a388d7-c654-426c-81ad-7ba61875ecf6.png)

- In the **Container to load balance** section, select **Add to load balancer**.Additional information labeled **api:3000** is shown.
- In the **api:3000** section, do the following:
    - For the **Production listener port** field, select **80:HTTP**.
    - For the **Target group name**, select your group: **api**.
    - Select **Next step**.
![x](https://user-images.githubusercontent.com/91766546/159162437-bd2754f9-b53d-471b-88ca-efa7d69295e7.png)

- On the **Set Auto Scaling** page, leave the default setting and select **Next step**.
- On the **Review** page, review the settings then select **Create Service**.
- After the service has been created, select **View Service**.

![xx](https://user-images.githubusercontent.com/91766546/159162764-42991018-ce54-4ebd-98b6-27bebc3c55b2.png)
![xxx](https://user-images.githubusercontent.com/91766546/159162786-c7edf81d-331c-4be6-ad59-2367e4380ce2.png)
![xxxx](https://user-images.githubusercontent.com/91766546/159162796-3ea57203-94ab-45ab-810b-d350e2b07007.png)
![xxxxx](https://user-images.githubusercontent.com/91766546/159162800-56f98650-39cc-4479-8a21-30e56e6def58.png)
![xxxxxx](https://user-images.githubusercontent.com/91766546/159162805-e2440562-4c9c-4ee1-9373-ade336825e06.png)

You now have a running service. It may take a minute for the container to register as healthy and begin receiving traffic.

Validate your deployment by checking if the service is available from the internet and pinging it.
**To Find your Service URL:**

- Navigate to the [Load Balancers](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:) section of the EC2 Console.
- Select your load balancer **demo**.
- In the **Description** tab, copy the DNS name and paste into a new browser tab or window.
- You should see the message **Ready to receive requests**.

![Screenshot from 2022-03-11 23-01-22](https://user-images.githubusercontent.com/91766546/159163231-c4aaabf9-6e87-4add-af7c-642ad4628d19.png)

![a](https://user-images.githubusercontent.com/91766546/159163263-48672c37-e221-4104-a0d3-fa8f1da55418.png)

ee Each Part of the Service: The node.js application routes traffic to each worker based on the URL. To see a worker, simply add the worker name api/[worker-name] to the end of the DNS Name as follows:

- http://*[DNS name]*/api/users
- http://*[DNS name]*/api/threads
- http://*[DNS name]*/api/posts

![aa](https://user-images.githubusercontent.com/91766546/159163370-ba672631-55ef-4c77-b5ec-a3e7fb9408f4.png)

You can also add a record number at the end of the URL to drill down to a particular record. For example: http://[DNS name]/api/posts/1 or http://[DNS name]/api/users/4

![aaa](https://user-images.githubusercontent.com/91766546/159163384-75fb51ca-3ba4-44e4-9c59-b706a19286fa.png)

![aaaa](https://user-images.githubusercontent.com/91766546/159163388-434ccf9d-5423-4c22-98c6-d008773d30ec.png)

![aaaaa](https://user-images.githubusercontent.com/91766546/159163396-f1c8988e-b01c-4c76-8e1b-592db8cc47b6.png)

## Module 3 - Break the Monolith

In this module, I will break the node.js application into several interconnected services and push each service's image to an Amazon Elastic Container Registry (Amazon ECR) repository.

### Architecture Overview
The final application architecture uses Amazon Elastic Container Service (Amazon ECS) and the Application Load Balancer (ALB).

![w](https://user-images.githubusercontent.com/91766546/159163659-ce17ef50-bec8-4edf-9c94-3cb97c6ae4d4.png)

a. Client: The client makes traffic requests over port 80.

b. Load Balancer: The ALB routes external traffic to the correct service. The ALB inspects the client request and uses the routing rules to direct the request to an instance and port for the target group matching the rule.

c. Target Groups: Each service has a target group that keeps track of the instances and ports of each container running for that service.

d. Microservices: Amazon ECS deploys each service into a container across an EC2 cluster. Each container only handles a single feature.

### Why Microservices?

There are several reasons why companies nowadays implement microservices:

Isolation of Crashes: Good microservice architecture means that if one micro piece of your service is crashing, then only that part of your service will go down. The rest of your service can continue to work properly.

Isolation for Security: When microservice best practices are followed, the result is that if an attacker compromises one service, they only gain access to the resources of that service, and cannot horizontally access other resources from other services without breaking into those services as well.

Independent Scaling: When features are broken out into microservices, then the amount of infrastructure and number of instances used by each microservice class can be scaled up and down independently.

Development Velocity: Developers can be confident that any code they write will actually not be able to impact the existing code at all unless they explicitly write a connection between two microservices.

### Implementation Instructions

In the previous modules, I deployed my application as a monolith using a single service and a single container image repository. In this module, I will break the monolith, meaning I will deploy the application as three microservices, therefore I will need to provision three repositories (one for each service) in Amazon ECR.

The three services are:

1. users
2. threads
3. posts

Let's create repository for each service in Amazon ECR.

![ww](https://user-images.githubusercontent.com/91766546/159164332-01bdb185-994c-43ad-8b26-5c9cb9d266c9.png)

![www](https://user-images.githubusercontent.com/91766546/159164336-4af24616-58ec-4502-ba15-f2ab7c5c3faf.png)


You will need access to Docker to build and push the images for each service. If you are working on this project at different points in time, you may have been logged out of Docker. If this is the case, take the following steps to log into Docker again.

- Run *$(aws ecr get-login --no-include-email --region [your-region])*Replace *[your-region]*, for example: *$(aws ecr get-login --no-include-email --region us-west-2)*

If the authentication was successful, you will receive the confirmation message: **Login Succeeded**.

![q](https://user-images.githubusercontent.com/91766546/159166447-20168c86-80bf-4df6-8003-c432b77d25c3.png)

In the project folder **amazon-ecs-nodejs-microservices/3-microservices/services**, you will have folders with files for each service. Notice how each microservice is essentially a clone of the previous monolithic service.

You can see how each service is now specialized by comparing the file **db.json** in each service and in the monolithic api service. Previously posts, threads, and users were all stored in a single database file. Now, each is stored in the database file for its respective service.

Open your terminal, and set your path to *~/amazon-ecs-nodejs-microservices/3-microservices/services*

![qq](https://user-images.githubusercontent.com/91766546/159166469-ee620539-99df-4c86-a4a8-91fb1f9ca5e5.png)

**Build and Tag Each Image**

- In the terminal, run *docker build -t [service-name] ./[service-name]*Replace the *[service-name]*, for example: *docker build -t posts ./posts*
- After the build completes, tag the image so you can push it to the repository:*docker tag [service-name]:latest [account-ID].dkr.ecr.[region].amazonaws.com/[service-name]:v1*Replace *[service-name]*, *[account-ID]*, and *[region]*, for example: *docker tag posts:latest [account-id].dkr.ecr.us-west-2.amazonaws.com/posts:v1*
- Push your image to ECR: docker push [account-id].dkr.ecr.[region].amazonaws.com/[service-name]:v1Replace *[service-name]*, *[account-ID]*, and *[region]*.

If you navigate to your ECR repository, you should see your images tagged with v1.

Repeat these steps for each microservice image.

**NOTE:** Be sure to build and tag all three images.

![qqq](https://user-images.githubusercontent.com/91766546/159166488-06fc3854-1469-452c-b239-fdc79e233246.png)

![qqqq](https://user-images.githubusercontent.com/91766546/159166574-571dcf97-e9a6-48d3-9caa-55decf750ce0.png)

![qqqqq](https://user-images.githubusercontent.com/91766546/159166598-ccb83211-bef6-4abf-ad23-85ce06813ab5.png)

![qqqqqq](https://user-images.githubusercontent.com/91766546/159166603-6ab068f0-4d3f-4ffd-ba7d-a5e8a5ce96af.png)

![qqqqqqq](https://user-images.githubusercontent.com/91766546/159166610-009a91a8-8679-4074-82bd-b5545bb0d696.png)

![qqqqqqqq](https://user-images.githubusercontent.com/91766546/159166616-791686ca-1127-4abd-a718-f51bad2160f2.png)

## Module 4 - Deploy Microservices

In this module, I will deploy the node.js application as a set of interconnected services behind an Application Load Balancer (ALB). Then, I will use the ALB to seamlessly shift traffic from the monolith to the microservices. 

### Architecture Overview

This architecture will deploy the microservices and safely transition the application's traffic away from the monolith.

![s](https://user-images.githubusercontent.com/91766546/159166793-27c2c320-f93d-4700-84dd-d47784cc0306.png)

1. Switch the Traffic This is the starting configuration. The monolithic node.js app running in a container on Amazon ECS.
2. Start Microservices Using the three container images you built and pushed to Amazon ECR in the previous module, you will start up three microservices on your existing Amazon ECS cluster.
3. Configure the Target Groups Like in Module 2, you will add a target group for each service and update the ALB Rules to connect the new microservices.
4. Shut Down the Monolith By changing one rule in the ALB, you will start routing traffic to the running microservices. After traffic reroute has been verified, shut down the monolith.

he step-by-step instructions below will deploy the microservices.

I will deploy three new services to the cluster that I launched in Module 2. Like Module 2, I will write Task Definitions for each service.

To speed things up, in this next step I will create task definitions for each service (users, threads, posts) from the Amazon ECS console by writing them as JSON.

Here is the JSON template:

![q1](https://user-images.githubusercontent.com/91766546/159175762-3e03ab7e-9d12-4bdd-85cd-03215cb6b77a.png)
Follow these steps below:

From the Amazon Container Services console, under Amazon ECS, select Task definitions.

In the Task Definitions page, select the Create new Task Definition button.

In the Select launch type compatibility page, select the EC2 option and then select Next step.

In the Configure task and container definitions page, scroll to the Volumes section and select the Configure via JSON button.

Copy and paste the following code snippet into the JSON field, replacing the existing code.Remember to replace the [service-name], [account-ID], [region], and [tag] placeholders.

![ss](https://user-images.githubusercontent.com/91766546/159176556-42322ce7-2d6f-4e78-8cf8-2a4d9e0e4eae.png)
![sss](https://user-images.githubusercontent.com/91766546/159176566-6682ad87-fd1c-4045-9f1c-cf1ac769ef9a.png)
![ssss](https://user-images.githubusercontent.com/91766546/159176570-8f5c3e10-e3bd-4e1a-9712-c6f219485749.png)
![sssss](https://user-images.githubusercontent.com/91766546/159176576-6a28ecf1-92a4-4594-a390-6bcd49dde092.png)
![ssssss](https://user-images.githubusercontent.com/91766546/159176581-72f3c8d2-f24c-467e-9255-fa0717204c26.png)
![sssssss](https://user-images.githubusercontent.com/91766546/159176587-4e28be54-395d-40b6-8c99-e34ede9dd3b3.png)
![ssssssss](https://user-images.githubusercontent.com/91766546/159176610-e5b0b214-71d5-4b6f-b3a0-682d4fdce015.png)
![sssssssss](https://user-images.githubusercontent.com/91766546/159176612-09b9b226-cb80-4090-83f9-0b3152f03a40.png)
![ssssssssss](https://user-images.githubusercontent.com/91766546/159176618-1b4e2158-b563-4b83-81f1-0f869cda7cfe.png)
![sssssssssss](https://user-images.githubusercontent.com/91766546/159176622-8328b8cc-bdca-4d72-8026-976b189c6cb9.png)
![ssssssssssss](https://user-images.githubusercontent.com/91766546/159176624-2e89df98-e747-4244-a424-bfb08deb3b33.png)

As in Module 2, configure a target group for each service (posts, threads, and users). A target group allows traffic to correctly reach a specified service. I will configure the target groups using AWS CLI. However, before proceeding, ensure the correct VPC name that is being used for this project:

Navigate to the Load Balancer section of the EC2 Console.
Select the checkbox next to demo, select the Description tab, and locate the VPC attribute (in this format: vpc-xxxxxxxxxxxxxxxxx). Note: You will need the VPC attribute when you configure the target groups.

![r0](https://user-images.githubusercontent.com/91766546/159177040-2d6ac6f0-88db-4a5e-a87d-ac29709ac454.png)

![r](https://user-images.githubusercontent.com/91766546/159177054-ae57346c-e8a3-4f98-8368-bfd73c80c0e1.png)

![r1](https://user-images.githubusercontent.com/91766546/159177061-dd1db34d-7996-4123-86a8-1e69954dd340.png)

![rr](https://user-images.githubusercontent.com/91766546/159177068-25317945-7683-4a48-b51c-ed10f8f7b889.png)

![rrr](https://user-images.githubusercontent.com/91766546/159177074-b030337a-7716-43d5-b39a-8fe02977403e.png)

![rrrr](https://user-images.githubusercontent.com/91766546/159177078-97052407-2e9c-40f2-8d6f-3a6840fbaea3.png)

The [listener](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html) checks for incoming connection requests to your ALB in order to route traffic appropriately.

Right now, all four of your services (monolith and your three microservices) are running behind the same load balancer. To make the transition from monolith to microservices, you will start routing traffic to your microservices and stop routing traffic to your monolith.

**Access the listener rules**

- Navigate to the [Load Balancer section of the EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Locate the Load Balancer named **demo** and select the checkbox next to it to see the Load Balancer details.
- Select the **Listeners** tab.

![rrrrr](https://user-images.githubusercontent.com/91766546/159177086-b18ed22f-243b-4369-a771-96b36dc203af.png)

There should only be one listener listed in this tab. Take the following steps to edit the listener rules:

- Under the **Rules** column, select **View/edit rules**.
- On the **Rules** page, select the plus (**+**) button.The option to **Insert Rule** appears on the page.
- Use the following rule template to insert the necessary rules which include one to maintain traffic to the monolith and one for each microservice:
    - IF Path = /api/[service-name]* THEN Forward to [service-name]For example: IF Path = /api/posts* THEN Forward to posts
    - Insert the rules in the following order:
        - api: */api** forwards to *api*
        - users: */api/users** forwards to *users*
        - threads: */api/threads** forwards to *threads*
        - posts: */api/posts** forwards to *posts*
- Select **Save**.
- Select the back arrow at the top left corner of the page to return to the load balancer console.
![t](https://user-images.githubusercontent.com/91766546/159177400-02e448d2-e980-476a-8cdb-3a6ee3335f52.png)

![tt](https://user-images.githubusercontent.com/91766546/159177408-4463dcd5-88d9-4a00-a541-989a67ed267b.png)

![ttt](https://user-images.githubusercontent.com/91766546/159177410-094351ed-af70-427b-9fbc-63c0a6d1ae3e.png)

![tttt](https://user-images.githubusercontent.com/91766546/159177415-77a999c0-23ca-4175-bd54-ecd4400c066d.png)

![ttttt](https://user-images.githubusercontent.com/91766546/159177420-970e9ca7-e92a-4162-a4e5-7523e44494ca.png)

Deploy the three microservices (posts, threads, and users) to your cluster. Repeat these steps for each of your three microservices:

- Navigate to the [Amazon ECS console](https://console.aws.amazon.com/ecs/home) and select **Clusters** from the left menu bar.
- Select the cluster **BreakTheMonolith-Demo**, select the **Services** tab then select **Create**.

![u](https://user-images.githubusercontent.com/91766546/159178455-4c6e3f43-6fd5-4893-a668-15ac590815b0.png)

- On the **Configure service** page, edit the following parameters (and keep the default values for parameters not listed below):
    - For the **Launch type**, select **EC2**.
    - For the **Task Definition**, select the **Enter a value** button to automatically select the highest revision value.For example: api:1
    - For the **Service name**, enter a service name (*posts, threads, or users*).
    - For the **Number of tasks**, enter *1*

![uu](https://user-images.githubusercontent.com/91766546/159178466-532ba8e9-9ebe-458a-9991-d9f0ddd55d96.png)

- Select **Next step**.
- On the **Configure network** page, **Load balancing** section, do the following:
    - For the **Load balancer type**, select **Application Load Balancer**.
![uuu](https://user-images.githubusercontent.com/91766546/159178491-0c043b59-727e-4ac7-bae7-5135cf2a649d.png)

    - For the **Service IAM role**, select **BreakTheMonolith-Demo-ECSServiceRole**.
    - For the **Load balancer name**, verify that **demo** is selected.
    - In the **Container to load balance** section, select the **Add to load balancer** button and make the following edits:
        - For the **Production listener port**, set to **80:HTTP**.
        - For the **Target group name**, select the appropriate group: *(***posts**, **threads**, or **users**)
- Select **Next step**.

![uuuu](https://user-images.githubusercontent.com/91766546/159178502-a18617c0-4e14-46df-927d-0ce9845c8d06.png)

- On the **Set Auto Scaling** page, select **Next step**.
- On the **Review** page, select **Create Service**.
![uuuuu](https://user-images.githubusercontent.com/91766546/159178511-9cb1c5a3-1d26-4a5a-98fa-707ae22665ef.png)

- Select **View Service**.
![uuuuuu](https://user-images.githubusercontent.com/91766546/159178532-0d26bd63-db37-4d4b-927a-d9ab7b85180d.png)

![uuuuuuu](https://user-images.githubusercontent.com/91766546/159178612-c9bbc793-b6d8-4a01-8869-117dfd8a8b87.png)

It should only take a few seconds for all your services to start. Double check that all services and tasks are running and active before you proceed.

![uuuuuuuu](https://user-images.githubusercontent.com/91766546/159178618-eb6907b1-197c-44a0-8aa7-01bfc5c3448c.png)

Your microservices are now running, but all traffic is still flowing to your monolith service. To reroute traffic to the microservices, take the following steps to update the listener rules:

- Navigate to the [Load Balancers section of the EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Select the checkbox next to your load balancer to see the Load Balancer details.
- Select the **Listeners** tab.There should only be one listener listed.
- Under the **Rules** column, select **View/edit rules**.
![uuuuuuuuu](https://user-images.githubusercontent.com/91766546/159178659-79bfea3b-90dd-442c-aea2-1298e117b311.png)

- On the **Rules** page, select the minus (****) button from the top menu.
- Delete the first rule (*/api* forwards to api*) by selecting the checkbox next to the rule.
- Select **Delete**.

![uuuuuuuuuu](https://user-images.githubusercontent.com/91766546/159178739-9be2d461-88dd-443c-9058-4d43327f7991.png)

- Update the default rule to forward to drop-traffic:
    - Select the edit (pencil) button from the top menu.
    - Select the edit (pencil) icon next to the default rule (**HTTP 80: default action**).
    - Selec the edit (pencil) icon in the **THEN** column to edit the **Forward to**.
    - In the **Target group** field, select **drop-traffic**.
    - Select the **Update** button.

![uuuuuuuuuuu](https://user-images.githubusercontent.com/91766546/159178755-5460982b-9a62-4010-8ec5-290a1659a7d0.png)

![uuuuuuuuuuuu](https://user-images.githubusercontent.com/91766546/159178768-8faf3320-aa99-47fb-bd1f-00788479d9ef.png)


![uuuuuuuuuuuuu](https://user-images.githubusercontent.com/91766546/159178785-486b7a52-23ea-468b-b06c-bfbe3a4ad510.png)

![uuuuuuuuuuuuuu](https://user-images.githubusercontent.com/91766546/159178792-e3b4d34a-bbe3-4356-95ca-52ff31583c9f.png)

**Disable the monolith:** With traffic now flowing to your microservices, you can disable the monolith service.

- Navigate back to the Amazon ECS cluster **BreakTheMonolith-Demo-ECSCluster**.
- In the **Services** tab, select the checkbox next to **api** and select **Update**.
- On the **Configure service** page, locate **Number of tasks** and enter *0*.
![Screenshot from 2022-03-12 01-10-03](https://user-images.githubusercontent.com/91766546/159178883-3421ef23-a37b-49a6-8c8e-e2525216f951.png)
![Screenshot from 2022-03-12 01-10-46](https://user-images.githubusercontent.com/91766546/159178899-37ca6ea9-22e0-4aeb-aeb7-4c2d90f4940f.png)

- Select **Skip to review**.
- Select **Update Service**.

![Screenshot from 2022-03-12 01-11-17](https://user-images.githubusercontent.com/91766546/159178911-c2bf9568-7fc9-4197-ae60-fc2a88d2d597.png)

See the values for each microservice:  To see each service, simply add the service name to the end of your DNS name:

http://[DNS name]/api/users

![aa](https://user-images.githubusercontent.com/91766546/159178988-a6848268-9dc6-4f88-8594-0719d6d65285.png)

![aaaa](https://user-images.githubusercontent.com/91766546/159178995-b12df555-2f30-4a0c-911f-5b3adf85a699.png)

![aaaaa](https://user-images.githubusercontent.com/91766546/159179006-6f503781-fc36-41ca-ab45-646777694c32.png)


## Module Five - Clean Up

In this module, I will terminate the resources I created during this project. I will stop the services running on Amazon ECS, delete the ALB, and delete the AWS CloudFormation stack to terminate the ECS cluster, including all underlying EC2 instances.

Start clean up by deleting each of the services (posts, threads, and users) that are running in my cluster:

Navigate to the Amazon ECS console and select Clusters.
- In the **Services** tab, select a service and then select **Delete**.
- Confirm the deletion.
- Repeat the steps until all the services are deleted.

Before proceeding to the next step, you nned to either wait for all running tasks to terminate or select the **Tasks** tab and select **Stop All**.

Repeat these steps for each of your services on the cluster.
![d](https://user-images.githubusercontent.com/91766546/159179737-7dc7da42-d7cd-4f8f-9fa9-07ed9dd9d523.png)
![dd](https://user-images.githubusercontent.com/91766546/159179743-b724312e-2cb0-4eaa-a8dc-f2e5fb7b9a1a.png)
![ddd](https://user-images.githubusercontent.com/91766546/159179751-84d682fc-3d00-4741-b870-17b4d90814e7.png)

Navigate to the [Load Balancer section of the EC2 Console.](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:). Select the **Listeners** tab.
![dddd](https://user-images.githubusercontent.com/91766546/159179847-01391aae-ab88-47a4-99b8-469b1434a30d.png)

- Select the listener, then select **Delete**.
![ddddd](https://user-images.githubusercontent.com/91766546/159179860-0fb75a11-3a2c-4ec3-bf70-ccc8b8e562c7.png)

- Confirm the deletion.Navigate to [Target Groups](https://console.aws.amazon.com/ec2/v2/home?#TargetGroups:) in the EC2 console.Check the checkbox at the top of the list (next to **Name**) to select all target groups.Select **Actions** then select **Delete**.Confirm the deletion.
![dddddd](https://user-images.githubusercontent.com/91766546/159179874-7e83fa65-67d0-4643-a2e8-b8a5aef5838d.png)
![ddddddd](https://user-images.githubusercontent.com/91766546/159179900-0ba547f0-81b7-484b-a670-593ac3ccd372.png)

Navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home?).

- Check the box next to the Cloudformation stack **BreakTheMonolith-Demo**.
- Select **Actions** then select **Delete Stack**.
- Confirm the deletion.
- The stack status should change to DELETE_IN_PROGRESS*.*

**⚠** **WARNING!** Leaving a stack running will result in charges on your AWS account.

![dddddddd](https://user-images.githubusercontent.com/91766546/159179959-db5319b0-f994-4d09-8428-fd27dae37416.png)

![ddddddddd](https://user-images.githubusercontent.com/91766546/159179969-c1ecb1f4-7a68-4cf1-af10-eb1fe59646a4.png)

Navigate to [Task Definitions](https://console.aws.amazon.com/ecs/home?#/taskDefinitions) in the Amazon ECR console.

- Select a task definition (api, posts, threads, or users).
- On the **Task Definition Name** page, select the checkbox next to the task name.
- Select **Actions** then from the drop-down list select **Deregister**.
- Confirm your action.

Repeat these steps for all four task definitions.

Navigate to [Repositories](https://console.aws.amazon.com/ecs/home?#/repositories) in the Amazon ECR console.

- Select the checkbox next to a repository and then select **Delete**.
- Confirm the deletion.
- Repeat the steps until all the repositories are deleted.

![dddddddddd](https://user-images.githubusercontent.com/91766546/159180080-47f3882e-1ac8-440e-a18e-959cf714ae02.png)
![ddddddddddd](https://user-images.githubusercontent.com/91766546/159180092-c518751e-8f26-4f05-930e-6eb4bac5c9c2.png)


## Congratulations!!! You completed the project and split a monolithic app into containerized microservices using Amazon Web Services (AWS).
