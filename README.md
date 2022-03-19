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


![ff](https://user-images.githubusercontent.com/91766546/159133486-05bc991e-abdb-4b0c-998c-48ac4b2f3795.png)

![fff](https://user-images.githubusercontent.com/91766546/159133497-bf57801b-c230-4293-b354-49d5d0e20196.png)

![ffff](https://user-images.githubusercontent.com/91766546/159133505-21339514-0d48-4543-8de3-0da50f24fe59.png)

![fffff](https://user-images.githubusercontent.com/91766546/159133514-b6ca8688-cd29-41e6-b210-b79f3528a5f5.png)

![Screenshot from 2022-03-11 22-10-36](https://user-images.githubusercontent.com/91766546/159133617-e33eaf01-80a5-4db2-a815-0f45be901e6d.png)

![Screenshot from 2022-03-11 22-10-48](https://user-images.githubusercontent.com/91766546/159133652-b31008af-c04f-4e6f-b2bd-d4eb2bc8668b.png)

![p8](https://user-images.githubusercontent.com/91766546/159133547-09b78824-0ac2-480d-9cab-3fedbc0244d2.png)


![Screenshot from 2022-03-11 22-16-25](https://user-images.githubusercontent.com/91766546/159133664-b8da9b39-15b6-4b0b-ab26-50d85cc52bfb.png)

