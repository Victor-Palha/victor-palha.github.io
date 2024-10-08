---
layout:
title: "The Docker Guide - All you need to know about Docker before starting"
date: 2024-10-08 12:18:20 -0000
categories: [Documentation]
tags: [guide, docker]
---

# Docker
First of all, let's understand what Docker is and why it is so important in the world of software development.
All examples and explanations are based on the official Docker documentation and my personal experience and the examples that i show here you can find at my github repository [Docker Guide](https://github.com/Victor-Palha/Guia-completo-de-Docker)

## What is Docker?
* **Docker** is a software platform that **simplifies the setup process** for applications;
* It allows us to configure **containers**, which act like servers to run our applications;
* With Docker, we can easily create **independent environments** that work on various operating systems;
* It also ensures that projects remain **highly performant**.

Docker containers provide a lightweight, standardized unit of software that packages up the code and all its dependencies, so the application runs quickly and reliably across computing environments. This consistency helps avoid common issues caused by differences in development, testing, and production environments.

---

## Why Docker?
* **Docker** significantly **speeds up** the setup process for developers' environments;
* Containers require **minimal maintenance** as they run exactly as configured;
* **Performance** is superior to traditional virtual machines (VMs), making applications more efficient;
* It eliminates the **"matrix from hell"** scenario, where compatibility issues across different environments make it difficult to manage and deploy applications smoothly.

By using Docker, developers can run multiple isolated applications on the same system without interference, and it's easy to scale and replicate environments for testing, development, or production.

---

## Which version to use?
* Docker is divided into two versions: **Docker CE** and **Docker EE**;
* **Docker CE** (Community Edition) is the **free** version, allowing users to leverage Docker for most use cases;
* **Docker EE** (Enterprise Edition) is the **paid** version aimed at businesses. It offers enhanced stability, certified versions, and official **support** from the Docker team.

For most development needs, **Docker CE** is sufficient, but **Docker EE** is ideal for enterprises needing advanced security, compliance, and support guarantees.

---

## Installation
You can find detailed instructions for installing Docker on different operating systems in the official documentation.

- [Docker Documentation](https://docs.docker.com/)
- [Windows Installation Guide](https://docs.docker.com/desktop/install/windows-install/)
- [Linux Installation Guide](https://docs.docker.com/desktop/install/linux-install/)
- [Mac Installation Guide](https://docs.docker.com/desktop/install/mac-install/)

Make sure to follow the steps appropriate for your platform to get Docker up and running smoothly.

---

## Testing Docker
* Let's test Docker by using a **real image** (we'll explain what that means shortly);
* To run any containers, we use the command `docker run`;
* This command allows us to **pass various parameters** to configure the container;
* In this example, we will only pass the name of the image, which is `docker/whalesay` (we'll explain what this is shortly);
* We'll also use a command called `cowsay` with a message.

Here’s how you can run it:
```bash
    docker run docker/whalesay cowsay "Hello-World"
```
- This will run a Docker container that prints a fun "whale" message to the console, giving you a quick test of Docker's functionality.
```bash
_____________ 
< Hello-World >
 -------------
    \
     \
      \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/
```

---

# Containers
## What are Containers?
* A **container** is a **package of code that can execute an action**, such as running an application built in Node.js, PHP, Python, etc.;
* In other words, our projects will run inside the containers that we create or use;
* **Containers utilize images** to be executed, which serve as the blueprint or template for creating the containers;
* **Multiple containers can run simultaneously**, allowing different services to operate together—for example, one container for PHP and another for MySQL.

Containers provide a lightweight and efficient way to package applications and their dependencies, ensuring that they run consistently across different environments. This isolation enables developers to work on and deploy applications without worrying about conflicts or dependencies affecting other parts of the system.

---

## Container vs. VM (Virtual Machine)
* A **container** is an application designed for a specific purpose; it does not have its own operating system, and its size typically ranges from a few megabytes;
* A **VM** has its own operating system and can be several gigabytes in size; it can run multiple functions simultaneously;
* Containers are more resource-efficient to run due to their specific use case, as they share the host operating system's kernel and run in isolated user spaces;
* VMs consume more resources because they require a full operating system stack, but they are capable of running a wider range of applications and services concurrently.

### Key Differences:
- **Resource Usage**: Containers are lightweight and use fewer system resources compared to VMs, which require more overhead to run the complete OS.
- **Speed**: Containers start up almost instantly, while VMs can take a few minutes to boot up, given their larger size.
- **Isolation**: While containers provide process-level isolation, VMs provide complete isolation since each VM has its own OS.

In summary, while both containers and VMs have their unique strengths, containers are more suitable for microservices and applications that require quick deployment and scalability, whereas VMs are better for running multiple operating systems and complex applications that need full isolation.

---

## Container and Image
* **Image and Container** are fundamental resources in Docker;
* An image is the **"project"** that will be executed by the container; it contains all the instructions needed to create and run the container;
* A container is **Docker running an image**, and thus executing the code specified by that image;
* The flow is: we create an image and run it using a container.

---

## Where to Find Images?
* We can find images in the Docker repository, known as **[Docker Hub](https://hub.docker.com)**;
* On this site, we can **check which images are available** for the technology we are looking for, such as Node.js;
* We can also **learn how to use them** effectively;
* To execute an image in a container, we use the command: `docker run <image>`.

Here’s an example:

```bash
    # Example:
    docker run ubuntu
    # Note that it will download the image and then exit immediately because there's nothing to execute.
    # To check the containers that are currently running, refer to the next section.
```

When you run the command above, it will pull the Ubuntu image from Docker Hub and attempt to start a container. Since no command is specified for the Ubuntu container to execute, it will terminate right away. This illustrates how to pull an image and highlights the need to specify what you want to do within the container.

---

## Checking Running Containers
* The command **`docker ps`** or **`docker container ls`** shows which containers are currently running;
* By using the **-a flag**, we can also see all containers that have ever run on the machine;
* This command is useful for **understanding what is currently running and happening** in our environment.

```bash
    # Running containers:
    docker ps
    # All containers that have executed:
    docker ps -a
```

---

## Running a Container with Interaction
* We can run a container and keep it **active in the terminal**;
* To do this, we use the **-it flag**, which allows us to **execute commands available in the container** we are running;
* We can use the Ubuntu image for this.

```bash
docker run -it ubuntu
```

By running this command, you'll enter an interactive shell within the Ubuntu container. From there, you can execute any commands just like you would on a regular Ubuntu system. This is particularly useful for development, debugging, and testing purposes, as it allows direct interaction with the containerized environment.

---

## Running a Container in the Background
* When we start a container that persists, **it occupies the terminal**;
* To run a container in the background, allowing us to avoid having multiple terminal tabs open, we use the **-d flag** (detached mode);
* We can also check **background containers using `docker ps`**;
* For this example, we can use the **nginx** image:

```bash
    docker run -d nginx
```

By running this command, the nginx container will start in the background, freeing up your terminal for other tasks. You can then use `docker ps` to see that the nginx container is running without being tied to the terminal session. This is particularly useful for running services like web servers, databases, or any long-running applications in a more efficient manner.

---

## Exposing Ports
* Docker **containers do not have connections to anything outside of themselves** by default;
* Therefore, we need to expose ports, which is done using the **-p flag**.
* Why expose ports?
    * This allows external traffic to reach the service running within the container;
    * For example, if we run a web server inside a container, we need to expose port 80 to access it from the host machine.

```bash
docker run -d -p 80:80 nginx
```

* This way, the **container will be accessible on port 80**;
* Why use `80:80`?
    * The first `80` is the port we want to expose from the container;
    * The second `80` is the port we want to access on the host machine;
    * In other words, we could run the command `docker run -d -p 8080:80 nginx`, allowing us to access the container through port `8080`, which reflects port `80` of the nginx server.

For example:
- Accessing `localhost:8080` will redirect to `localhost:80`, where the nginx server is running inside the container. This port mapping allows external traffic to reach the service running within the container, facilitating communication between the containerized application and the outside world.

---

## Stopping Containers
* If you are following the tutorial, you should have some containers running;
* You can check which containers are currently running with the command `docker ps`;
* To stop a container, we use the command `docker stop <container ID or container name>`;
    * You can view this information with the `docker ps` command;
* This way, you will be freeing up resources that were being used by that container.

Here’s how it works:

```bash
docker ps
# Output
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES       
ecc1f9240d54   nginx     "/docker-entrypoint.…"   7 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp   quirky_kalam

docker stop ecc1f9240d54

docker ps
# Output
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
```

In this example:
1. The first `docker ps` command lists the running containers, showing the container ID, image name, command, creation time, status, ports, and container name.
2. We then stop the nginx container using its container ID (`ecc1f9240d54`).
3. Running `docker ps` again confirms that the container is no longer running, which means it has been successfully stopped and the resources it was using have been freed. This helps maintain an efficient environment by managing resource usage effectively.

Certainly! Here’s the information split into more readable sections:

---

## Restarting Containers
* We learned how to stop a container using `docker stop`. To restart a container, we can use the command `docker start <id>`.
* Remember that **the `run` command always creates a new container**.
* So, if you need to reuse an existing container, opt for `start`.

### Finding the Container
To find the container you want to restart, you can list all containers (including those that have exited):

```bash
docker ps -a
```
**Example Output:**
```
CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS                        PORTS                    NAMES
ecc1f9240d54   nginx                       "/docker-entrypoint.…"   4 minutes ago    Exited (0) 4 minutes ago                               quirky_kalam
e7ad4bd564fb   ubuntu:latest               "/bin/bash"              46 minutes ago   Exited (127) 43 minutes ago                            bold_elbakyan
8cd0a9f9675a   docker/whalesay             "cowsay Hello-World"     2 hours ago      Exited (0) 2 hours ago                                 hungry_chaum
```

### Starting the Container
To start the previously stopped container, use:

```bash
docker start ecc1f9240d54
```
**Example Output:**
```
ecc1f9240d54
```

### Checking if the Container is Running
To verify if the container is now running, execute:

```bash
docker ps
```
**Example Output:**
```
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS          PORTS                  NAMES
ecc1f9240d54   nginx     "/docker-entrypoint.…"   5 minutes ago   Up 20 seconds   0.0.0.0:8080->80/tcp   quirky_kalam
```

### Summary
1. Use `docker ps -a` to find the container you want to restart.
2. Use `docker start <container_id>` to start the container.
3. Use `docker ps` to confirm that the container is running and accessible.

---

## Setting a Container Name
* We can define a container name using the **`--name` flag**;
* If we don’t specify a name, **we receive a random name**, which can be problematic for a production application;
* The flag is included with the `docker run` command.

```bash
docker run -it --name linux-melhor-que-windows ubuntu
```

---

## Checking Logs
* We can **check what happened in a container** using the `logs` command;
* This is done by using the following format: `docker logs <container_id or container_name>`;
* We can also use the `-f` flag to **follow the logs in real time**.

```bash
docker logs linux-melhor-que-windows
```

### Summary
1. Use `docker logs <container_name>` to view the logs of the specified container, which can help diagnose issues or understand the container's activity.
2. Use the `-f` flag with the logs command to continuously stream the log output in real time, allowing you to monitor the container’s activity as it happens.

---

## Removing Containers
* We can **remove a container from the machine** where Docker is running;
* The command is `docker rm <id>`;
* If the container is still running, we can use the `-f` flag to force the removal;
* Once removed, the container will no longer be listed in `docker ps -a`.

### Example Usage
1. **List all containers** to find the one you want to remove:

```bash
docker ps -a
```
**Example Output:**
```
CONTAINER ID   IMAGE                       COMMAND                  CREATED             STATUS                        PORTS                    NAMES
fba6f58d722e   ubuntu                      "/bin/bash"              6 minutes ago       Exited (0) 4 minutes ago                               linux-melhor-que-windows
ecc1f9240d54   nginx                       "/docker-entrypoint.…"   20 minutes ago      Exited (0) 4 minutes ago                               quirky_kalam
e7ad4bd564fb   ubuntu:latest               "/bin/bash"              About an hour ago   Exited (127) 59 minutes ago                            bold_elbakyan
8cd0a9f9675a   docker/whalesay             "cowsay Hello-World"     2 hours ago         Exited (0) 2 hours ago                                 hungry_chaum
```

2. **Remove the container** using its ID:

```bash
docker rm fba6f58d722e
```
**Example Output:**
```
fba6f58d722e
```

3. **Verify the removal** by listing all containers again:

```bash
docker ps -a
```
**Example Output:**
```
CONTAINER ID   IMAGE                       COMMAND                  CREATED             STATUS                         PORTS                    NAMES
ecc1f9240d54   nginx                       "/docker-entrypoint.…"   21 minutes ago      Exited (0) 5 minutes ago                                 quirky_kalam
e7ad4bd564fb   ubuntu:latest               "/bin/bash"              About an hour ago   Exited (127) About an hour ago                            bold_elbakyan
8cd0a9f9675a   docker/whalesay             "cowsay Hello-World"     2 hours ago         Exited (0) 2 hours ago                                   hungry_chaum
```

### Summary
1. Use `docker rm <container_id>` to remove a specific container.
2. If a container is running and you want to remove it, use `docker rm -f <container_id>` to force its removal.
3. After removal, verify that the container no longer appears in the list with `docker ps -a`, ensuring it has been successfully deleted from the system.

---

# Docker Images

## What is an Image?
* Images are **derived from files that we program** for Docker to create a structure that executes specific actions in containers.
* They contain information such as _base images_, _base directories_, _commands to be executed_, _application ports_, etc.
* When running a container based on the image, **the instructions are executed in layers**.

---

## How to Choose a Good Image
* We can download images from [Docker Hub](https://hub.docker.com/).
* However, **anyone can upload an image**, which can be a concern.
* We should pay attention to **official images**.
* Other interesting parameters are the **number of downloads** and the **number of stars**.

---

## Creating an Image
You can find the source code for the examples below in the [NodeDocker Repository](https://github.com/Victor-Palha/Guia-completo-de-Docker/tree/main/NodeDocker)

* To create an image, we need a _Dockerfile_ in a folder where the project will reside.
* This file will require some instructions to be executed:
  * `FROM`: base image;
  * `WORKDIR`: application directory;
  * `EXPOSE`: application port;
  * `COPY`: which files need to be copied into the container;

### Example: Creating a Simple Node.js Application
1. Create a folder, open your code editor, and follow these instructions:

```bash
mkdir node-app && cd node-app
npm init -y
npm install express
touch app.js
```

2. Inside `app.js`, write the following code:

```javascript
const express = require('express');

const app = express();

app.get('/', (req, res) => {
    res.send('Hello World from Docker');
});

app.listen(8080, () => {
    console.log('Server running on port 8080');
});
```

3. Now, let's create our Docker image:

```bash
touch Dockerfile
```

### Dockerfile Example
```Dockerfile
FROM node               # Base image

WORKDIR /app            # Application directory inside the container

COPY package*.json .    # Copy package.json and package-lock.json to the application directory

RUN npm install         # Install dependencies

COPY . .                # Copy files to the container 

EXPOSE 8080             # Application port

CMD ["node", "app.js"]  # Command to be executed
```

---

## Running an Image
* To run the image, we first need to **build it**.
* The command is `docker build <path to Dockerfile>`.
* We can use the `-t` flag to **set a name for the image**.
* Then, we use `docker run <image>` to execute the image.

```bash
docker build -t express_server . # Assuming the terminal is in the same directory as the Dockerfile

docker run -p 3000:8080 express_server
```

Now you can access the application by going to `localhost:3000` in your browser.

---

## Modifying an Image
* Whenever we change the code of an image, **we need to build it again**.
* For Docker, it is as if it’s a **completely new image**.
* After building, we will run it with another unique ID created using `docker run`.

---

## Image Layers Cache concept
* Docker images are divided into **layers**.
* Each instruction in the Dockerfile **represents a layer**.
* When something is updated, **only the layers after the updated line are rebuilt**.
* The rest remains cached, making the **build process faster**.

---

## Downloading Images
* We can **download an image** from the hub and have it available in our environment.
* Use the command `docker pull <image>`.
* This way, if used in another container, **the image will already be ready for use**.

---

## Learning More About Commands
* All Docker commands have access to a **--help flag**.
* Using this flag, **we can see all available options for the commands**.
* This is useful for recalling something or performing a different task with the same command.

```bash
# Example
docker run --help
```

---

## Multiple Applications, Same Container
* We can initialize **multiple containers with the same image**.
* The applications will run in parallel.
* To test this, we can assign a **different port** for each and run in **detached mode**.

---

## Renaming an Image and Tag
* We can **name the image** we created.
* Use the command `docker tag <name>` for this.
* We can also **modify the tag**, which is like a version of the image, similar to Git.
* To set the tag, use: `docker tag <name>:<tag>`.

```bash
docker images
# Output
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE  
express_server       latest    7ae03f51fdc1   39 minutes ago   1.1GB 

docker tag express_server:latest express_server:1.0.0

docker images
# Output
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE  
express_server       1.0.0     7ae03f51fdc1   40 minutes ago   1.1GB 
express_server       latest    7ae03f51fdc1   40 minutes ago   1.1GB 
```

---

## Removing Images
* Just like containers, **we can remove images with a command**.
* The command is `docker rmi <id or image name>`.
* Images that are being used by a container will produce an error in the terminal.
* We can use the **-f flag** to force the removal.

```bash
docker images
# Output
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE  
express_server       1.0.0     7ae03f51fdc1   47 minutes ago   1.1GB 
express_server       latest    7ae03f51fdc1   47 minutes ago   1.1GB 

docker rmi -f 7ae03f51fdc1
```

---

## Removing Images and Containers
* With the command `docker system prune`, we can **remove unused images, containers, and networks**.
* The system will require confirmation to perform the removal.

```bash
docker system prune
```

---

## Automatically Removing Containers After Use
* A container can be automatically deleted after its use.
* To do this, we use the `--rm` flag in the `docker run` command.
* The command `docker run --rm <image>` will execute the container and then delete it.
* This way, **we don't have to worry about deleting the container** after its use.

```bash
docker run --rm ubuntu
```

---

## Copying Files Between Containers
* To copy files between containers, we use the command: `docker cp`.
* It can be used to copy a file from a directory to a container, or from a container to a directory.
* Open two terminals for testing:

### Terminal 1 (Docker Prompt)
```bash
docker run -it --rm ubuntu
```

### Terminal 2 (Your PC Prompt)
```bash
mkdir testeToCopy
cd testeToCopy
touch teste.txt
cd ..

docker ps
# Output
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
d5c6e1111edf   ubuntu    "/bin/bash"   2 minutes ago   Up 2 minutes             musing_gould

docker cp ./testeToCopy d5c6e1111edf:.  
# Explanation:
# ./testeToCopy -> directory we want to copy
# d5c6e1111edf -> id of the container we want to copy to
# . -> directory where we want to paste

# We can also copy files from a container to a directory
docker cp d5c6e1111edf:./testeToCopy/test.txt .
# Explanation:
# d5c6e1111edf -> id of the container we want to copy from
# ./testeToCopy/test.txt -> file we want to copy
# . -> directory where we want to paste
```

---

## Checking Processing Information
* To check execution data of a container, we use the command `docker top <id or name of container>`.
* This way, we can access when it started, process ID, command description, etc.

---

## Checking Container Data
* To check various information such as **id, creation date, image, and much more**, we use the command `docker inspect <id or name of container>`.
* This way, we can access a wealth of information about the container.

---

## Checking Processing
* To check the processes running in a container, we use the command: `docker stats`.
* This way, we can access the status of processing and memory used.

---

## Authentication on Docker Hub
* To conclude this step, we will need an account on Docker Hub.
* To authenticate via the terminal, we use the command `docker login`.
* Then, enter your username and password.
* Now we can **send our own images** to the Hub.

```bash
docker login
## Output
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: <your username>
Password: <your password>
```

---

## Sending an Image to Docker Hub
* To send our image to Docker Hub, we use the command `docker push <image>`.
* However, we first need to **create the repository** for the image on the Hub website.
* After creating the repository, we need to **rename the image** so that it is sent to the correct repository:
  * `docker tag <image> <username>/<repository name>:<tag>`;
* It is also necessary to **be authenticated**.
* After sending the image, we can download it using the command `docker pull <image>`.

---

## Logging Out of Docker Hub
* To remove the connection between our machine and Docker Hub, we use the command `docker logout`.
* Now we cannot send images anymore because we are not authenticated.

```bash
docker logout
```

---

## Sending Image Updates
* To send an update, **we will first build the image again**.
* **Changing the image tag** to the new version.
* **Then we will push it** again to the repository.
* This way, all versions will be available for use.

---

## Downloading and Using the Image
* To download the image, we can use the command `docker pull <image>`.
* After downloading, we can use the command `docker run <image>`.
* This way, the image will be executed in a container.

---

# Volumes

## What are volumes?
* **Volumes** provide a way to persist data outside of containers, ensuring that data remains available even if a container is deleted or restarted.
* Any data created within a container is **stored inside the container itself**, and when the container is removed, the data is lost.
* Volumes help manage data more efficiently and facilitate **easy backups** and **data sharing** between containers.

---

## Types of Volumes
There are three main types of volumes in Docker:

1. **Anonymous Volumes**: 
   * Created automatically when the `-v` flag is used without specifying a name.
   * These volumes are given a random name and are primarily used for temporary or one-time data storage.
   
2. **Named Volumes**: 
   * Volumes with an explicit name that you define, making them easier to manage and refer to.
   * Useful when you need to reuse volumes across multiple containers or when you want to back up data.
   
3. **Bind Mounts**: 
   * Instead of being managed by Docker, **bind mounts** allow you to map a directory on your host machine to a directory inside a container.
   * This provides direct access to files on the host system from within the container, which is useful for local development.

---

## The Data Persistence Problem
When you create a container from an image, any files generated inside that container are stored **within the container's file system**. If the container is deleted, the data is lost along with it. This is where volumes come in—they allow you to persist data independently of the container lifecycle.

Let's walk through an example using PHP to demonstrate this.
You can find the source code for the examples below in the [ProjectVolume Repository](https://github.com/Victor-Palha/Guia-completo-de-Docker/tree/main/project-volume)

### Example Setup with PHP:
1. Create a new project directory:
   ```bash
   mkdir project-volume
   cd project-volume
   ```

2. Create a `Dockerfile`:
   ```bash
   touch Dockerfile
   ```

   Add the following content to the `Dockerfile`:
   ```Dockerfile
   # Dockerfile for PHP application
   FROM php:8-apache

   WORKDIR /var/www/html

   COPY . .

   EXPOSE 80

   RUN chown -R www-data:www-data /var/www
   ```

3. Add files for a basic PHP app:
   ```bash
   touch index.php process.php
   mkdir messages
   ```

4. Add basic HTML form to `index.php`:
   ```php
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Messages</title>
   </head>
   <body>
       <h1>Write your message:</h1>
       <form action="process.php" method="POST">
           <input type="text" name="message" placeholder="Write...">
           <button type="submit">Submit</button>
       </form>
   </body>
   </html>
   ```

5. Handle form submission and save data in `process.php`:
   ```php
   <?php
   $message = $_POST["message"];
   $files = scandir("./messages");
   $num_files = count($files) - 2; // accounting for '.' and '..'

   $fileName = "msg-{$num_files}.txt";
   $file = fopen("./messages/{$fileName}", "x");
   fwrite($file, $message);
   fclose($file);

   header("Location: index.php");
   ?>
   ```

Now let's build the Docker image and run the container:

```bash
docker build -t php-volume:1.0 .
docker run -d -p 80:80 --name php-container php-volume:1.0
```

You can now open `localhost:80` in your browser, submit messages, and they will be saved inside the container. However, if you remove the container, the messages will be lost unless you use a volume.

---

## Anonymous Volumes
Anonymous volumes can be created like this:

```bash
docker run -v /data -d -p 80:80 --name php-container php-volume:1.0
```
Here, `/data` is the directory inside the container linked to an anonymous volume. You can view the created volumes with:

```bash
docker volume ls
```

---

## Named Volumes
Named volumes are more manageable because you can easily reference them by name:

```bash
docker run -v phpvolume:/var/www/html -d -p 80:80 --name php-container php-volume:1.0
```

This links the `/var/www/html` directory inside the container to a named volume `phpvolume`. Even if the container is removed, the data persists in this named volume, and you can reuse it by creating another container:

```bash
docker stop php-container
docker run -v phpvolume:/var/www/html -d -p 81:80 --name php-container2 php-volume:1.0
```

Accessing `localhost:81/messages/msg-0.txt` will show that the message you previously submitted is still there.

---

## Bind Mounts
Bind mounts allow you to map a directory on your host machine to a container directory, providing direct access to files:

```bash
docker run -v "/path/on/host:/path/in/container" -d -p 80:80 --name php-container php-volume:1.0
```

For example:

```bash
docker run -v "/home/user/project-volume/messages:/var/www/html/messages" -d -p 80:80 --name php-container php-volume:1.0
```

This maps the `messages` directory from your local machine to the container, allowing files to be shared directly.

---

## Live Project Updates with Bind Mounts
Bind mounts are also useful for **live updates** to your project without needing to rebuild the container. You can simply map the entire project directory:

```bash
docker run -v "/path/to/project:/var/www/html" -d -p 80:80 --name php-container --rm php-volume:1.0
```

Any changes you make to your local files will be instantly reflected in the container. For example, updating `index.php`:

```php
<h2>Hello, Docker!</h2>
```

After refreshing the browser, the changes will be visible.

---

## Managing Volumes

### Creating a Volume:
You can create a named volume manually using:

```bash
docker volume create myvolume
```

Then use this volume when running a container:

```bash
docker run -v myvolume:/var/www/html -d -p 80:80 --name php-container php-volume:1.0
```

### Listing Volumes:
To list all volumes, use:

```bash
docker volume ls
```

To inspect a specific volume:

```bash
docker volume inspect myvolume
```

### Removing Volumes:
You can remove a volume (as long as it’s not in use by a container) with:

```bash
docker volume rm myvolume
```

To remove all unused volumes:

```bash
docker volume prune
```

### Read-only Volumes:
You can create read-only volumes for containers where you don’t want the container to write to the volume:

```bash
docker run -v myvolume:/data:ro -d -p 80:80 --name php-container php-volume:1.0
```

In this case, `:ro` means **read-only**, ensuring that the container cannot modify the data.

# Networks

## What are Docker Networks?
* Docker networks provide a way to **manage container connections** with each other, the host system, or external platforms.
* Like volumes, networks are **created separately from containers**, allowing flexible network configurations.
* There are various **network drivers** that enable different types of container communications, which we will discuss shortly.
* Networks make communication between containers simple and efficient.

## Types of Connections
Docker containers typically communicate in three main ways:
1. **External connection**: Connecting to an external API or remote server.
2. **Host connection**: Communicating with the host machine running Docker.
3. **Container-to-container communication**: Using the **bridge driver**, containers can connect and communicate with each other.

## Network Drivers
There are several network drivers in Docker:
1. **Bridge**: The default and most commonly used network driver, suitable for container-to-container communication.
2. **Host**: Enables direct communication between a container and the host machine.
3. **Macvlan**: Assigns a unique MAC address to a container for network communication.
4. **None**: Removes all network access from the container.
5. **Plugins**: Allows the use of third-party network extensions to create custom network configurations.

## Listing Networks
To list all networks in your Docker environment, use:
```bash
docker network ls
```
Some default networks are preconfigured when Docker is installed.

## Creating Networks
You can create a new network using:
```bash
docker network create <network-name>
```
By default, this will create a **bridge** network, which is the most common type used.

You can also create networks with other drivers:
```bash
docker network create -d macvlan my-macvlan-network
```
After creation, you can list and inspect the networks:
```bash
docker network ls
```

## Removing Networks
To remove a network, use:
```bash
docker network rm <network-name>
```
Note that **the network must not be in use by any containers**. You can use the `-f` flag to force network removal.

## Pruning Networks
You can remove all unused networks with:
```bash
docker network prune
```
This will delete all networks that are not connected to at least one container:
```bash
WARNING! This will remove all networks not used by at least one container.
Are you sure you want to continue? [y/N] y
```

## External Connection
Containers can **freely connect to the outside world**, such as accessing an open API. Let’s test this with an example Flask application.
You can find the source code for the examples below in the [ExternalConnection Repository](https://github.com/Victor-Palha/Guia-completo-de-Docker/tree/main/external-connection)

### Example Setup:
1. Create a new directory and add a `Dockerfile`:
   ```bash
   mkdir external-connection
   cd external-connection
   touch Dockerfile
   ```
2. Define a Flask-based API in the `Dockerfile`:
   ```Dockerfile
   FROM python:latest

   RUN apt-get update -y && apt-get install -y python3-pip python3-dev

   WORKDIR /app

   RUN pip install flask requests

   COPY . .

   EXPOSE 5000

   CMD ["python3", "./app.py"]
   ```
3. Add a basic Flask app in `app.py` that fetches random user data:
   ```python
   import flask
   import requests

   app = flask.Flask(__name__)

   @app.route('/', methods=['GET'])
   def index():
       data = requests.get('https://randomuser.me/api')
       return data.json()

   if __name__ == "__main__":
       app.run(host="0.0.0.0", port=5000)
   ```

4. Build and run the container:
   ```bash
   docker build -t flask-server .
   docker run -d -p 5000:5000 --name flask-container flask-server
   ```

You can now access the app at `http://localhost:5000/` in your browser.

## Host Connection
Containers can also **communicate with the host** by referring to the special hostname `host.docker.internal`. This allows the container to access services or resources running on the host machine.

## Container-to-Container Communication
Let’s now connect two separate containers to demonstrate **container-to-container communication**. For this example, one container will run a Flask app and the other will run MySQL. 

### Project Structure:
```bash
external-connection/
├── flask-server/
│   ├── app.py
│   └── Dockerfile
└── mysql-server/
    ├── Dockerfile
    └── schema.sql
```

### MySQL Setup
1. In `mysql-server/Dockerfile`, define the MySQL image:
   ```Dockerfile
   FROM mysql:5.7

   COPY schema.sql /docker-entrypoint-initdb.d

   EXPOSE 3306

   VOLUME ["/backup/"]
   ```

2. In `mysql-server/schema.sql`, define the database schema:
   ```sql
   CREATE DATABASE flaskdocker;
   USE flaskdocker;

   CREATE TABLE users (
       id INT AUTO_INCREMENT,
       name VARCHAR(255) NOT NULL,
       PRIMARY KEY (id)
   );
   ```

3. Build the MySQL image:
   ```bash
   cd mysql-server
   docker build -t mysql-server-network .
   ```

### Create the Docker Network
```bash
docker network create api-network
```

### Run the MySQL Container:
```bash
docker run -d -p 3306:3306 --name mysql-container --network api-network -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql-server-network
```

### Update Flask App to Use MySQL
In the `flask-server/app.py`, update the app to interact with the MySQL database:
```python
import flask
import requests
from flask_mysqldb import MySQL

app = flask.Flask(__name__)

# MySQL configurations
app.config['MYSQL_HOST'] = 'mysql-container'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = ''
app.config['MYSQL_DB'] = 'flaskdocker'

mysql = MySQL(app)

@app.route('/', methods=['GET'])
def index():
    data = requests.get('https://randomuser.me/api')
    return data.json()

@app.route("/inserthost", methods=['POST'])
def inserthost():
    data = requests.get('https://randomuser.me/api').json()
    username = data['results'][0]['name']['first']

    cur = mysql.connection.cursor()
    cur.execute("INSERT INTO users(name) VALUES(%s)", (username,))
    mysql.connection.commit()
    cur.close()

    return username

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

### Build and Run Flask Container:
```bash
cd flask-server
docker build -t flask-server-network .
docker run -d -p 5000:5000 --name flask-container --network api-network flask-server-network
```

The Flask app is now connected to the MySQL container, and you can interact with both services.

## Connecting a Container to an Existing Network
You can connect a container to an existing network using:
```bash
docker network connect <network-name> <container-name>
```
For example:
```bash
docker network connect api-network flask-container
```

## Disconnecting a Container from a Network
To disconnect a container from a network, use:
```bash
docker network disconnect <network-name> <container-name>
```

With Docker networks, you have great flexibility in controlling how your containers communicate, both internally and with the outside world.

# Conclusion
## 1. **Images:**
   - **What are Docker Images?** 
     Docker images are immutable, read-only templates that define the application’s environment, including the OS, application code, dependencies, and libraries. Images are the foundation for containers and can be built from a Dockerfile.
   - **Layers in Images:** 
     Docker images are composed of multiple layers, each representing changes like adding a file or installing a package. These layers allow efficient storage and reuse, as Docker caches layers for faster builds.
   - **Building Docker Images:** 
     You can create images using a `Dockerfile`, a text file that contains all the instructions needed to build the image, such as base image, file copying, environment variables, and execution commands.
   - **Managing Images:** 
     Docker provides commands to list, inspect, tag, and remove images. You can also push images to remote registries (like Docker Hub) or pull images created by others.

---

## 2. **Containers:**
   - **What are Containers?**
     Containers are the runnable instances of Docker images. They provide a lightweight, isolated environment where your application runs. While images are static, containers are dynamic and can be started, stopped, paused, or removed.
   - **Running Containers:**
     Containers are created from images and can be started with specific runtime parameters like ports, volumes, networks, and environment variables. They can be run in detached mode (`-d`) or attached to the terminal.
   - **Stateless and Ephemeral Nature:** 
     Containers are designed to be stateless, meaning they can be removed and recreated without affecting the overall application state, especially when using external volumes and networks to persist data.
   - **Managing Containers:**
     Docker provides powerful commands to start, stop, inspect, list, and remove containers. Containers can also be named and connected to networks or volumes dynamically.

---

## 3. **Volumes:**
   - **What are Volumes?** Volumes ensure data persistence across container restarts by providing a mechanism for containers to store and share data outside of their own file systems.
   - **Types of Volumes:**
     - **Anonymous Volumes**: Created without a name, often for temporary use.
     - **Named Volumes**: Explicitly named and can be easily referenced across containers.
     - **Bind Mounts**: Maps a directory on the host machine to a container directory for real-time data sharing.
   - **Managing Volumes:** Volumes can be created, inspected, and removed independently from containers, allowing data to persist even after containers are destroyed.

---

## 4. **Networks:**
   - **What are Docker Networks?** Networks enable communication between containers, the host, and external services. They allow containers to interact while maintaining isolation from other network resources.
   - **Types of Network Drivers:**
     - **Bridge:** Default driver used for container-to-container communication.
     - **Host:** Allows containers to share the host’s networking stack.
     - **Macvlan:** Assigns MAC addresses to containers, making them appear as physical devices on the network.
     - **None:** Disconnects containers from all networks.
   - **Managing Networks:** Docker allows you to list, create, and remove networks. Containers can be connected or disconnected from these networks dynamically, offering flexibility in application architecture.

---

## 5. **Real-World Use Cases:**
   - **Persistent Storage with Volumes:** Implementing a PHP application where user messages are persisted using named volumes, showcasing Docker’s data retention capabilities.
   - **Building and Running Containers:** Using Dockerfiles to create custom images (e.g., for Flask and MySQL applications) and running these images as containers. This demonstrates how applications and databases can run in isolated containers while sharing data through volumes.
   - **Inter-Container Communication:** Utilizing Docker networks, particularly the **bridge** driver, to connect a Flask application and a MySQL database running in separate containers. This illustrates how containers interact within a shared network, making it easy to build scalable, distributed microservices.

---

## 6. **Pruning and Maintenance:**
   - Docker makes it easy to prune unused volumes, networks, images, and containers to free up system resources and maintain a clean, efficient environment.

---

## Conclusion:
Docker’s **Images, Containers, Volumes, and Networks** form the backbone of containerized applications. **Images** are reusable templates that define the environment and application code, while **containers** are the runtime instances of these images, providing lightweight and isolated environments for application execution. **Volumes** ensure data persistence and easy sharing across containers, while **networks** manage the communication between containers and external systems. Together, these components allow for modular, scalable, and highly efficient development, deployment, and maintenance of applications in a modern cloud or local development environment. By leveraging these Docker features, developers can build complex architectures, ensuring applications are easily deployable and maintainable.

