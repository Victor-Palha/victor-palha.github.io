---
layout:
title: "The Docker Guide - Docker Swarm and AWS"
date: 2024-10-08 17:18:20 -0000
categories: [Documentation]
tags: [guide, docker, swarm, aws, devops]
---

# Docker Swarm and AWS
## What is Container Orchestration?
* Orchestration is the process of **managing and scaling** the containers of an application.
* It involves having **a central service that governs other services**, ensuring they are functioning correctly.
* This ensures the application is healthy, available, and can handle load effectively.
* Some popular orchestration tools include **Docker Swarm, Kubernetes, and Apache Mesos**.

### Concepts of Container Orchestration:

#### 1. **Docker Swarm**:
   - Docker Swarm is Docker's native clustering and orchestration tool. It allows you to create a cluster of Docker hosts and run containers across them as a single pool of resources.
   - **Features of Docker Swarm**:
     - **Scalability**: You can scale up or down the number of containers based on demand.
     - **Load Balancing**: Swarm automatically distributes traffic between containers in the cluster.
     - **Fault Tolerance**: If a node fails, Swarm can restart containers on another node.
     - **Decentralized Design**: Every node in a Swarm cluster is capable of performing orchestration and management tasks.
   
#### 2. **AWS (Amazon Web Services)**:
   - AWS provides various services for container orchestration, such as **Amazon ECS (Elastic Container Service)** and **Amazon EKS (Elastic Kubernetes Service)**.
   - **Amazon ECS**: A fully managed service that makes it easy to run and scale containerized applications. It supports both Docker and AWS Fargate, which allows you to run containers without managing servers.
   - **Amazon EKS**: A managed Kubernetes service that allows you to run Kubernetes clusters without managing the control plane.
   - **Benefits of AWS for Orchestration**:
     - **Scalability**: AWS services like ECS and EKS can scale containerized applications automatically based on usage.
     - **Integration**: AWS offers seamless integration with other AWS services like S3, IAM, and CloudWatch for monitoring and security.
     - **High Availability**: AWS services run on a global infrastructure that ensures high availability and fault tolerance.

#### 3. **Comparison: Docker Swarm vs Kubernetes**:
   - **Ease of Use**: Docker Swarm is easier to set up and use for smaller projects, while Kubernetes is more feature-rich but comes with greater complexity.
   - **Community and Ecosystem**: Kubernetes has a larger community and ecosystem, with broader adoption across the industry.
   - **Scalability**: Kubernetes is better suited for large, complex applications requiring advanced scaling and orchestration features.
   - **Flexibility**: Kubernetes offers more advanced networking, storage options, and resource management.

Container orchestration, whether through **Docker Swarm**, **Kubernetes**, or services like **AWS ECS/EKS**, is essential for modern applications that require scaling, high availability, and fault tolerance. These tools automate the deployment, management, and scaling of containers, ensuring applications run smoothly across multiple environments. Choosing the right orchestration tool depends on the complexity of the application, the scale of the deployment, and the specific needs of the project.

---

## What is Docker Swarm?
* Docker Swarm is a **native Docker tool** for **container orchestration**.
* It allows for **horizontal scaling** of projects in a straightforward manner.
* It's the well-known **Cluster** system!
* The **advantage of Swarm** over other orchestrators is that the commands are very similar to Docker's existing commands, making it easier to adopt.
* Every Docker installation includes Swarm, but we need to **initialize the Swarm** for orchestration.

### Key Concepts:
1. **Nodes**: These are instances (machines) that participate in the Swarm.
2. **Manager Node**: A node responsible for managing other nodes within the Swarm.
3. **Worker Node**: Nodes that perform tasks assigned by the Manager Node.
4. **Service**: A group of tasks that the Manager Node instructs the Worker Nodes to execute.
5. **Task**: A specific command or operation that is executed on the nodes.

#### Docker Swarm in Action:
   - Once Swarm is initialized, you can deploy services across a cluster of machines, which allows for easy scaling. 
   - **Horizontal Scaling** in Docker Swarm means that you can increase the number of replicas of a service, distributing them across multiple nodes.
   - The **Manager Node** takes care of orchestrating the deployment, ensuring that the required number of replicas is always running.

#### Fault Tolerance and High Availability:
   - Docker Swarm ensures **fault tolerance** by distributing services across different nodes. If a node fails, Swarm will reschedule the tasks on a healthy node, ensuring service continuity.
   - **High Availability**: With a properly configured cluster, Docker Swarm can ensure that services remain available even if individual nodes go down.

#### Simplicity of Use:
   - Docker Swarm is known for its ease of setup compared to more complex orchestrators like Kubernetes. This makes it ideal for smaller projects or when you're already familiar with Docker commands.
   - **Commands** like `docker service create`, `docker swarm init`, and `docker node ls` are intuitive, and Docker users can quickly grasp them.

Docker Swarm is a simple, yet powerful container orchestration tool, built right into Docker. With its **horizontal scaling** capabilities, **fault tolerance**, and easy-to-understand commands, it’s a great choice for managing smaller clusters. It enables rapid deployment and scaling of services, making it an excellent choice for developers looking for seamless integration with Docker.

## Initializing Docker Swarm

To properly demonstrate Docker Swarm, we need multiple _nodes_, meaning we require **more machines** to create a cluster environment. There are two main solutions for obtaining these machines:

1. **AWS (Amazon Web Services)**: 
   - You can create an AWS account and launch some servers to act as nodes in your Docker Swarm cluster.
   - Although AWS requires a **credit card**, it offers a **free tier** that allows users to run virtual machines for free, within limits.
   - This is a scalable and flexible option if you're planning to build more complex infrastructures or need persistent nodes.

2. **Docker Labs**:
   - A completely **free** option that runs in your web browser.
   - You can experiment with Docker Swarm without setting up any cloud servers.
   - The only drawback is that it **expires after 4 hours**, so it's more suited for quick tests and learning rather than long-term projects.

### Use Cases for AWS and Docker Labs:
- **AWS**: If you're planning to set up a real-world production cluster or need long-running nodes to test complex scenarios, AWS is an excellent choice.
- **Docker Labs**: Ideal for rapid experimentation, learning, or small-scale, temporary demos. You can explore Swarm without worrying about infrastructure.

### AWS (Amazon Web Services)
For our example, we will use AWS because it closely relates to real-world projects. We will create three EC2 instances and configure them to work with Docker Swarm. You should have at least a basic understanding of how to set up infrastructure in AWS.

1. Create an AWS account;
2. Create three EC2 instances called **Node1, Node2, and Node3**;
3. Choose the **AMAZON LINUX 2023 AMI** image;
4. Choose the **t2.micro** machine type;
5. Configure a **Security Group** with the following rules:
   - HTTP: 80
   - SSH: 22
   - Docker: 2377
6. Create an SSH key and download the `.pem` file;
7. Access the instances via SSH;
8. Install Docker.

We will use the following command to access the instances via SSH:

```bash
# Access Node1
ssh -i "YourSSHKey.pem" ec2-user@ec2-your-instance-ip.compute.amazonaws.com
```

Once connected to the instance, install Docker by running the following commands:

```bash
## Inside Node1 on AWS
sudo yum update -y
sudo yum install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```

## Initializing Docker Swarm and Creating the Manager Node
While still inside **Node1**, we will initialize Docker Swarm and create the Manager Node.

- We can initialize the Swarm using the command `docker swarm init`;
- In some cases, we need to specify the IP address of the Manager Node. We can do this with the command `docker swarm init --advertise-addr <IP>`;
- This will make **Node1** the **Manager** of the Swarm, while the other nodes will become **Workers**.

```bash
# Node1
sudo docker swarm init --advertise-addr <IP> # <IP> is the instance's IP, which can be found in the AWS console

# At this point, it will generate a command like this:
# docker swarm join --token SWMTKN-etx-etx-etx <ip>:2377
# Save this command in a text file as we'll need it later.
``` 

This process establishes **Node1** as the **Swarm Manager** and prepares the other nodes to join as **Worker Nodes**.

## Listing Active Nodes
- You can check which nodes are currently active in the Swarm by using the command `docker node ls`;
- This will display the nodes and their statuses in the terminal;
- It allows you to **monitor what the Swarm is orchestrating** and verify the health and roles of each node;
- This command becomes increasingly useful as more services and nodes are added to the Swarm.

```bash
sudo docker node ls
``` 

This is essential for keeping track of the cluster’s state and ensuring that all nodes are functioning correctly.

## Adding New Nodes to the Swarm
- You can add new nodes to the Docker Swarm using the command `docker swarm join --token <TOKEN> <IP>:<PORT>`;
- The token is generated when the Swarm is initialized, and this new machine becomes a **Worker Node**;
- Any **Tasks** executed by the **Manager Node** are replicated across the Worker Nodes;
- The `join` command is provided when the Swarm is initialized. If you didn't save it, you can generate a new token using `docker swarm join-token worker` on the manager node.

Here’s how to add a second node (Node 2) to the Swarm:

```bash
# Access Node 2
ssh -i "YourSSHKey.pem" ec2-user@ec2-your-instance-ip.compute.amazonaws.com
```

- Inside Node 2 we'll install Docker
```bash
sudo yum update -y
sudo yum install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```
- Now, use the join command you saved earlier from the txt file
```bash
sudo docker swarm join --token <TOKEN> <ip>:2377
```

- Repeat this process for Node 3;
- Now, you have **3 Nodes**, with **1 Manager Node** and **2 Worker Nodes**.

## Deploying a New Service in Docker Swarm
- You can start a new service on the **Manager Node** using the command `docker service create --name <NAME> <IMAGE>`;
- This creates a new container managed by the Swarm, and the service will be automatically orchestrated;
- Let's test this using **Nginx**, ensuring that **port 80** is exposed so that the container is available externally:

```bash
# On Node 1 (Manager Node)
sudo docker service create --name nginxswarm -p 80:80 nginx
```

- To verify the running service, use the command `docker service ls` to see the active service managed by the Swarm;
- You can then access **Nginx** in your browser by entering the **public IP** of the instance where the Swarm is running. This confirms that the service is up and running successfully.

## Removing a Service in Docker Swarm
- You can remove a service from the Swarm using the command `docker service rm <NAME>`;
- This action stops the corresponding container and removes the service from the Swarm;
- It’s important to be cautious, as removing a service may stop critical components of your application;
- After removing the service, verify the changes by running `docker service ls` to ensure the service is no longer active.

Example:

```bash
# On Node 1 (Manager Node)
sudo docker service ls  # Check active services

sudo docker service rm nginxswarm  # Remove the Nginx service

sudo docker service ls  # Confirm removal of the service
```

This will stop the Nginx container and remove the service from the Swarm management.

## Replicating Services in Docker Swarm
- You can create a service with multiple replicas using the command `docker service create --name <NAME> --replicas <NUMBER> <IMAGE>`;
- This initiates a task that replicates the service across the Worker nodes, enabling **horizontal scaling**;
- This is where orchestration really comes into play, as the Swarm distributes and manages these replicas;
- You can check the status of the service replication with the command `docker service ls`.

Example:

```bash
# On Node 1 (Manager Node)
sudo docker service ls  # Check existing services

sudo docker service create --name nginxreplicas --replicas 2 -p 80:80 nginx  # Create Nginx service with 2 replicas

sudo docker service ls  # Verify the service and replicas
```

Once the service is replicated, Swarm will ensure it runs on two different Workers. You can check the running containers on each Worker node:

```bash
# On Node 2 (Worker Node)
sudo docker ps  # Check running containers
```

```bash
# On Node 3 (Worker Node)
sudo docker ps  # Check running containers
```

This will confirm that the service is distributed and running across the available Workers.

## Verifying Orchestration in Docker Swarm
- Let's remove a container from a **Worker Node** to observe how Swarm handles orchestration;
- This action will prompt Swarm to restart the container automatically, ensuring that the service remains available;
- It's important to note that we need to use the `--force` flag to remove the container;
- We'll access **Node 3** and remove the Nginx container.

Example:

```bash
# On NODE 3
sudo docker ps  # List the running containers

sudo docker rm -f <CONTAINER_ID>  # Force remove the Nginx container
```

- After removing the container, check if Swarm has restarted the container on **Node 3**. This process might take a few seconds.
  
```bash
sudo docker ps  # Check the list of running containers again
```

- You should see that the Nginx container is back up and running, demonstrating Swarm's capability to maintain the desired state of services and ensure high availability.

## Checking the Swarm
- We can verify the details of the Swarm that Docker is utilizing within the manager;
- Use the command: `docker info` to get this information;
- This will display various details about the Swarm, including the number of Nodes, whether the Swarm is active, how many services are running, and more.

Example:

```bash
# On Node 1
sudo docker info  # Display information about the Docker installation and Swarm status
```

- This command provides a comprehensive overview of the Swarm's state, helping you monitor its health and performance.

## Removing an Instance from the Swarm
- You can remove an instance from the Swarm using the command `docker swarm leave`;
- From this moment on, the Node will no longer be part of the Swarm;
- Note that the Node's status changes to **Down**;
- You can check this with the command `docker node ls`;
- In this example, we will remove **Node 3** from the Swarm.

```bash
# On Node 3
sudo docker swarm leave
```

Now, Node 3 is no longer part of the Swarm, and its status has changed to **Down**. Let's verify this using the command `docker node ls` on Node 1, the Manager.

```bash
# On Node 1
sudo docker node ls  # Check the active Nodes in the Swarm

sudo docker service ls  # Check the active services in the Swarm
ID             NAME            MODE         REPLICAS   IMAGE          PORTS
u8zky7gwe4jb   nginxreplicas   replicated   4/3        nginx:latest   *:80->80/tcp
```

- Note that the replica is still running.
- This is because the Swarm will replicate the service on another Node to ensure the service remains available.
- You can check the running containers with:

```bash
sudo docker ps
```

- You will notice that the container that was running on Node 3 is now running on another Node; this may vary depending on your Swarm configuration.
- In my case, it started running on Node 1, which is also hosting the Manager.

## Removing a Node
- You can remove a Node using the command `docker node rm <ID>`;
- **This way, the instance will no longer be considered a Node in the Swarm**;
- The container will continue to run on the instance, but it will no longer be managed by the Swarm;
- You need to use the `--force` flag to remove the Node.

```bash
# On Node 1
sudo docker node rm --force <ID>
``` 

This command effectively deregisters the specified Node from the Swarm, ensuring that it is no longer part of the orchestration system while allowing any running containers to remain operational independently.

## Inspecting Services
- You can inspect a service using the command `docker service inspect <NAME>`;
- This allows you to view detailed information about the service;
- You can see the number of replicas, the service ID, the container ID, and more.

```bash
# On Node 1
sudo docker service ls

sudo docker service inspect nginxreplicas
```

This command provides comprehensive insights into the service configuration and its current state, which can be invaluable for monitoring and troubleshooting in a Docker Swarm environment.

## Checking Containers Activated by the Service
- You can see which containers are running in a service with the command `docker service ps <NAME>`;
- This will give you a list of containers that are running under the service, as well as those that have stopped;
- This command is similar to `docker ps -a`, but it is specific to services.

```bash
# On Node 1
sudo docker service ps nginxreplicas
```

Using this command, you can monitor the status of all containers associated with a specific service, which is essential for maintaining the health and performance of your applications in a Docker Swarm environment.

## Running Compose with Swarm
* To run Compose with Swarm, we will use the _Stack_ commands;
* The command is: `docker stack deploy -c <FILE.yaml> <NAME>`;
* This will execute the **Compose file**;
* However, we are now in Swarm mode, allowing us to use Nodes as replicas;
* We will create a Compose file to deploy a service in the Swarm.
* Let's create a Compose file inside Node 1 our Manager Node.

```bash
# On Node 1
# First, let's stop the running service
sudo docker service rm nginxreplicas

# Now, let's create our Compose file
vim docker-compose.yaml
# Press "esc" + ":x!" to save and exit
cat docker-compose.yaml
```

* In the Compose file, we will place the following content:

```yaml
version: '3.3'
services:
  nginx:
    image: nginx
    ports:
      - 80:80
```

```bash
# On Node 1
# Deploy the stack using the Compose file
sudo docker stack deploy -c docker-compose.yaml nginxstack

# List the services to confirm deployment
sudo docker service ls
```

## Scaling Services
* We can create new replicas on the **Worker Nodes**;
* We can scale a service using the command `docker service scale <NAME>=<NUMBER>`;
* This way, Swarm will send a task to replicate the service on the new Workers;

```bash
# Scale the nginx service to 3 replicas
sudo docker service scale nginxstack_nginx=3
```

## Preventing a Service from Receiving More Tasks
* We can make a service **stop receiving tasks from the Manager**;
* To do this, we will use the command: `docker node update --availability drain <ID>`;
* The **drain** status indicates that the Node will not receive new tasks;
* We can revert the Node to **active** to return it to normal operation;
* This is useful for performing maintenance on a Node, ensuring that tasks are not sent while the Node is out of operation.
* **Important**: This command should be executed on the **Manager Node**, as it is responsible for orchestrating the Swarm. You can specify the ID of any Node, whether it is a Worker or a Manager.
* After setting a Node to **drain**, the Swarm will redistribute the tasks that were assigned to it, ensuring that the application continues to run without interruptions.

```bash
# Node 1 (Manager)
sudo docker node ls  # List the Nodes and their statuses

sudo docker node update --availability drain <ID>  # Set the Node to drain

sudo docker node ls  # Check the status of the Nodes again
```

* To revert and allow the Node to receive tasks again, you can use the command:

```bash
sudo docker node update --availability active <ID>  # Return the Node to active status
```

* By using this approach, you can perform maintenance on the Node without affecting the availability of the service in the Swarm.

## Updating Parameters
* We can update the configurations of our Nodes and Services;
* We will use the command: `docker service update --image <IMAGE> <SERVICE>`;
* This command allows us to change the image used by the service to a newer version or a different one altogether;
* Only the Nodes that are in the **active** status will receive updates;
* When you update a service, Swarm will take care of replacing the running containers with new ones based on the updated configuration, ensuring zero downtime if configured properly;
* **Important**: It’s recommended to test the new image in a separate environment before deploying it to production to ensure compatibility and stability.
* You can also use additional flags to modify other parameters, such as resource limits or environment variables, during the update process.

```bash
# Update the service with a new image
sudo docker service update --image <NEW_IMAGE> <SERVICE_NAME>
```

* After running the command, you can monitor the update process by using the command `docker service ps <SERVICE_NAME>` to see the status of the tasks being updated.

Here's the translated text with additional information:

## Creating a Network for Swarm
* The connection between instances uses a different driver, called **overlay**;
* We can first create the network using the command: `docker network create --driver overlay <NAME>`;
* This overlay network allows containers across multiple Docker hosts to communicate with each other, facilitating inter-service communication in a Swarm environment;
* After creating the network, we can create a service and add the flag `--network <NAME>` to include the service in that network;

```bash
# List existing services
sudo docker service ls

# Remove an existing service if needed
sudo docker service rm <ID>

# Create an overlay network named 'swarm'
sudo docker network create --driver overlay swarm

# Create a new service with 3 replicas on the 'swarm' network
sudo docker service create --name nginxreplicas --replicas 3 -p 80:80 --network swarm nginx
```

* **Note**: When using overlay networks, ensure that the Docker daemon is configured properly on each host to allow inter-host communication. This is particularly important in production environments;
* You can verify that the network was created successfully by using the command `docker network ls`, which will list all available networks, including the newly created overlay network.

Here's the translated text with additional information:

## Connecting a Service to a Network
* We can also connect services that are already running to a network;
* We will use the update command: `docker service update --network-add <NETWORK> <SERVICE>`;
* After updating, we can check the results using the **inspect** command;

```bash
# List existing services
sudo docker service ls

# Remove an existing service if needed
sudo docker service rm <ID>

# Create a new service named 'nginxreplicas' with 3 replicas
sudo docker service create --name nginxreplicas --replicas 3 -p 80:80 nginx

# List services again to verify the new service is created
sudo docker service ls

# Connect the running service to the 'swarm' network
sudo docker service update --network-add swarm nginxreplicas

# Inspect the service to confirm the network connection
sudo docker service inspect nginxreplicas
```

* **Note**: After connecting a service to a network, you may want to check its connectivity by running a command like `docker exec` to enter one of the containers and attempt to ping another service on the same network;
* This functionality is useful for scenarios where services need to be updated without downtime, enabling seamless addition of new networks to running services for better communication and organization;
* To remove a network from a service, you can use the `--network-rm` option in a similar way: `docker service update --network-rm <NETWORK> <SERVICE>`. 

Here's a conclusion summarizing the key points about Docker Swarm based on the information provided:

## Conclusion on Docker Swarm

Docker Swarm is a powerful orchestration tool integrated with Docker, designed to simplify the management and scaling of containerized applications. It enables users to deploy and manage clusters of Docker engines (nodes), providing high availability and load balancing across the services running within the cluster. 

### Key Features of Docker Swarm:
1. **Ease of Use**: With commands that closely resemble standard Docker commands, Docker Swarm allows users to scale applications horizontally with minimal learning curve.
2. **Node Management**: The architecture consists of Manager Nodes, which oversee the cluster and Worker Nodes that execute the assigned tasks. Users can easily add or remove nodes, enabling dynamic scaling of resources.
3. **Service Management**: Users can create, update, and remove services using straightforward commands, allowing for efficient orchestration. The ability to replicate services across multiple nodes ensures that applications remain available and resilient.
4. **Task Distribution**: Docker Swarm intelligently distributes tasks among available Worker Nodes, automatically handling failures and restarting containers to maintain the desired state.
5. **Network Configuration**: Swarm utilizes overlay networks to facilitate communication between containers across different hosts, supporting complex multi-service architectures.
6. **Service Scaling**: Users can easily adjust the number of replicas for services, ensuring that application performance scales with demand.
7. **Maintenance Modes**: The ability to temporarily drain nodes for maintenance ensures that service availability is maintained without disruption.

In conclusion, Docker Swarm provides a robust platform for container orchestration, offering a user-friendly interface, effective resource management, and seamless scaling capabilities. By leveraging Docker Swarm, developers and IT teams can build resilient applications that can adapt to varying workloads, ensuring a healthy and responsive deployment environment. 
