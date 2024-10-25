---
layout:
title: "SSH - Basic guide to how use SSH with Docker example"
date: 2024-10-24 04:04:20 -0000
categories: [Documentation]
tags: [guide, devops, ssh]
---

# Understanding the SSH Protocol and Its Role in Secure Communication

In the realm of secure communication over the internet, **SSH (Secure Shell)** stands as one of the most widely used protocols for secure data exchange, remote command execution, and network services tunneling. Its robust security and encryption mechanisms make it indispensable for system administrators, developers, and anyone needing secure remote access to servers. In this article, we’ll explore how the SSH protocol operates, focusing on its **client-server model** and the encryption technologies it employs to ensure both privacy and data integrity.

### The Client-Server Model

At the core of SSH lies the **client-server model**, a fundamental network architecture where communication is initiated by the client. In this setup, the **SSH client** requests a connection to the **SSH server**. The SSH server, usually running on a remote machine, listens for connection requests on port 22 by default (though this can be changed for security reasons).

The connection process begins when the SSH client establishes communication with the server, initiating a handshake that lays the groundwork for secure interaction. This handshake involves **public key cryptography**, where the identity of the SSH server is verified using a **public key** that’s matched against the client’s known hosts list. This step ensures that the client is indeed communicating with the correct server, mitigating risks like **man-in-the-middle attacks**.

### Public Key Cryptography and Authentication

During the connection phase, the SSH client and server exchange cryptographic keys. Typically, the server sends its **public key** to the client, which is used to authenticate the server’s identity. The client checks this key against its list of known servers, ensuring it’s communicating with a trusted machine.

For additional security, **mutual authentication** can be employed. This involves the client also presenting a public key, which the server uses to verify the client’s identity. Alternatively, **password-based authentication** can be used, but public key authentication is far more secure and common in practice, especially for automated or frequent connections.

### Symmetric Encryption for Secure Communication

Once the authentication phase is complete and the server’s identity is verified, the SSH protocol switches to **symmetric encryption** for data exchange. Unlike public key encryption, symmetric encryption uses a **shared secret key** to encrypt and decrypt data. The key is negotiated during the handshake, and both the client and server use this key for all further communications.

SSH employs **strong encryption algorithms** like AES (Advanced Encryption Standard) to ensure that the data transmitted between the client and server remains confidential, even if it’s intercepted by malicious actors.

### Integrity with Hashing Algorithms

In addition to encryption, SSH guarantees the **integrity** of the exchanged data using **hashing algorithms**. Hashes, which are cryptographic representations of data, allow both parties to verify that the data they’re sending and receiving has not been altered in transit.

Each packet of data transmitted over SSH is accompanied by a **message authentication code (MAC)**, a unique hash calculated from the data and a shared secret. The receiving end recalculates the hash and compares it to the MAC to confirm that the data is intact and authentic. If the hashes don’t match, it’s a sign that the data has been tampered with, and the connection is terminated.

### Key Advantages of SSH

- **Security**: SSH offers encryption, authentication, and integrity checks, making it highly secure for remote access and data exchange.
- **Flexibility**: It can tunnel other protocols, like HTTP or FTP, through secure SSH connections, allowing safe transmission of less secure data.
- **Automation**: SSH is widely used in scripts and automated systems, enabling secure, unattended operations such as backups or updates.

[Source](https://www.ssh.com/academy/ssh/protocol)


## Practical Example: Using SSH to Connect to an Ubuntu Docker Container for Deployment

In this example, we’ll create a **Docker container** running **Ubuntu**, set up an SSH server inside the container, and then connect to it using SSH. We’ll simulate a real-world DevOps scenario where you deploy an application to a remote environment—in this case, the Docker container.

### Step 1: Create and Run an Ubuntu Docker Container

First, let's pull the latest Ubuntu image and create a container. We will install the **OpenSSH server** inside the container to allow SSH connections.

1. Pull the Ubuntu image and run it interactively:

   ```bash
   docker run -it --name ubuntu-ssh -p 2222:22 -p 3000:3000 ubuntu
   ```

   This command will:
   - Pull the **Ubuntu** image.
   - Start a container named `ubuntu-ssh` in interactively mode (`-it`).
   - Map port `2222` on your local machine to port `22` inside the container (default SSH port).

   Now you’re inside the container’s terminal!

#### Step 2: Install and Configure SSH Inside the Container

To set up SSH inside the Ubuntu container:

1. **Update the package list** and install **OpenSSH Server**:

   ```bash
   apt-get update
   apt-get install -y openssh-server
   ```

2. Start the SSH server:

   ```bash
   service ssh start
   ```

3. Set a password for the **root** user (required for SSH login):

   ```bash
   passwd root
   ```

4. Install Nano and Edit the SSH configuration file to allow root login:

   ```bash
   apt-get install nano
   nano /etc/ssh/sshd_config
   ```

   Find the line that says `#PermitRootLogin` and change it to:

   ```bash
   PermitRootLogin yes
   ```

5. Restart the SSH service to apply the changes:

   ```bash
   service ssh restart
   ```

Now, your Docker container is ready to accept SSH connections on port 2222 (which maps to port 22 inside the container).

#### Step 3: Connect to the Docker Container via SSH

Now that the SSH server is set up inside the Docker container, you can connect to it from your local machine.
Start a new terminal session on your local machine and follow these steps:

1. Generate an SSH key (if you don’t already have one):

   ```bash
   ssh-keygen -t rsa -b 4096
   ```

2. Copy your SSH public key to the container:

   Since this is a local setup and the container allows password login for root, we can copy the key using:

   ```bash
   ssh-copy-id -p 2222 root@localhost
   ```

3. Connect to the Docker container via SSH:

   ```bash
   ssh -p 2222 root@localhost
   ```

   You’re now logged into the Ubuntu container via SSH!
   You can check the connection by running `ls -a` or `pwd` to see the container’s filesystem.


#### Step 4: Deploy the Application Using Git

Assuming your application code is hosted in a Git repository (e.g., GitHub or GitLab), you can now clone the repo into the Docker container. For this example, we’ll deploy a Node.js application and you can find the repository [here](https://github.com/Victor-Palha/ssh-docker-example)!

1. First lets install Git inside the container and then navigate to the directory where you want to deploy the app:

   ```bash
   apt-get install -y git
   cd /root
   ```

2. Clone the Git repository:

   ```bash
   git clone https://github.com/Victor-Palha/ssh-docker-example
   ```

3. Navigate into the cloned repository:

   ```bash
   cd ssh-docker-example
   ```

4. Install the application's dependencies:

   ```bash
   apt-get install -y nodejs npm
   npm install
   ```

5. Start the application:

   ```bash
   npm start
   ```

### Warning!
The above steps are for demonstration purposes only and may not be suitable for production environments. In a real-world scenario, you would use a more secure setup, such as **SSH keys** for authentication, **firewall rules** to restrict access, and **container orchestration tools** like **Kubernetes** or **Docker Swarm** for managing containers.
And remember to **never expose your SSH server to the public internet without proper security measures** in place!

## Conclusion
Knowing how to use SSH effectively is a crucial skill for anyone working with servers, containers, or remote environments. By understanding the SSH protocol, its encryption mechanisms, and practical applications, you can securely manage your systems and deploy applications with confidence. Whether you’re a developer, system administrator, or DevOps engineer, SSH is an essential tool in your arsenal for secure, remote communication.
But remember! The examples provided here are for educational purposes only. Always follow best practices and security guidelines when setting up SSH connections in production environments.