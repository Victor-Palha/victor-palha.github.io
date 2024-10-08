---
layout:
title: "The Docker Guide - YAML and Docker Compose"
date: 2024-10-08 17:18:20 -0000
categories: [Documentation]
tags: [guide, docker, compose, yaml]
---

# YAML

## What is YAML?
- **YAML** is a data serialization language, its full name is **YAML Ain't Markup Language**. 
- It is commonly used for configuration files, especially with **Docker Compose**, allowing developers to define and manage multi-container Docker applications.
- YAML is designed to be easy for humans to read and write, focusing on simplicity and clarity in configuration.

## Key Features of YAML
- **Human-readable**: YAML is designed to be as close to natural language as possible. Its syntax is clean and uncluttered, making it easier to read than JSON or XML.
- **Flexible and powerful**: It supports a wide range of data types like strings, integers, floats, lists, dictionaries (maps), and booleans. YAML can also represent complex data structures.
- **Widely adopted**: It's used in various fields like configuration management, infrastructure as code (e.g., Ansible), and cloud services (e.g., Kubernetes configurations).
- **Indentation-based structure**: YAML uses indentation to represent the hierarchy of data, which is a key difference from markup languages like XML.

## Creating Our First YAML File
- A YAML file generally consists of key-value pairs, where each key is followed by a colon `:`, and its value comes after it.
- Let's create a Python program that reads from a **YAML** file:
You can find the source code [here](https://github.com/Victor-Palha/Guia-completo-de-Docker/tree/main/YAML)

```bash
mkdir YAML
cd YAML
touch app.py
# If pip3 is not installed
sudo apt install python3-pip

pip3 install pyyaml
```

In `app.py`, we can read a YAML file like this:

```python
import yaml
if __name__ == "__main__":
    with open("test.yaml", "r") as stream:
        dictionary = yaml.safe_load(stream)

    for key, value in dictionary.items():
        print(f"{key}: {value}")
```

Now, create a `test.yaml` file with the following content:

```yaml
name: "Victor"
age: 20
```

To run the program, simply execute:

```bash
python3 app.py
```

The output will be:

```bash
name: Victor
age: 20
```

## Spacing and Indentation
- The end of a line signifies the end of an instruction, so **no semicolons** are required.
- Indentation in YAML is done using **spaces only**, not tabs, and each level of indentation represents a new block or hierarchy.
- **Spacing is mandatory** after the key declaration.
- Comments in YAML are created using `#`.

Let's see this in practice:

```yaml
# Invalid structure
name:"Victor"
age: 20
## Try running the Python program to see the error
```

```yaml
# Valid structure with proper indentation
object:
  version: 1.0
  date: 2020-01-01
  active: true

## Run the Python program to see the output

# Data types in YAML
# String
name: "Victor"
surname: "Hugo"

# Integer
age: 20

# Float
height: 1.80

# Boolean
active: Off  # or false

# Lists
items:
  - item1
  - item2
  - item3

# Dictionaries
dictionary:
  key1: value1
  key2: value2
  key3: value3

# Null value
nothing: null  # or ~
```

## Why Use YAML for Docker Compose?
YAML's simplicity makes it an ideal choice for configuration management. When using Docker Compose, YAML allows developers to define multiple services, networks, and volumes in a clean, structured way. Here is an example of a **docker-compose.yml** file that sets up two services (a web application and a database):

```yaml
version: "3"
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  database:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
```

In this file:
- `version` specifies the Docker Compose file format version.
- `services` defines the two containers to be launched (a web server and a database).
- `ports` and `environment` show how configurations are mapped to the containers.

This powerful configuration language simplifies managing complex environments while being readable and easy to write.

### Conclusion
YAML is a versatile and human-friendly language for writing configuration files. Its importance is seen in many tools, especially when working with cloud-native environments, CI/CD pipelines, and infrastructure-as-code solutions like Docker Compose. Mastering YAML allows developers to handle configurations more effectively and boost productivity.

# Docker Compose

## What is Docker Compose?
- **Docker Compose** is a tool used to define and run **multi-container Docker applications**.
- It simplifies the process of managing containers by providing a single configuration file to orchestrate the entire setup.
- With Docker Compose, we can execute **multiple containers** at once.
- In large projects, Docker Compose is **essential** for maintaining and deploying various services simultaneously.
- Compose allows multiple `build` and `run` operations to be executed with a single command.

## Installing Docker Compose on Linux
- **Linux users** may need to install Docker Compose separately.
- Follow the installation instructions on the [official Docker website](https://docs.docker.com/compose/install/).
- Docker Compose is essential for achieving the objectives of this section.

## Creating Our First Docker Compose File
- First, create a file called **docker-compose.yml** in the project root directory.
- This file will **coordinate the containers and images** and uses some common keys:
  - `version`: The version of Docker Compose.
  - `services`: Defines the services to be used.
  - `volumes`: Specifies the containers/services and volumes for the application.

### Example of a Docker Compose file:

```bash
mkdir docker-compose
cd docker-compose
touch docker-compose.yml
docker compose version
docker compose --help
```

```yaml
version: '3.3' # Compose version

services:  # Services to be used
  db:  # Service name for MySQL
    image: mysql:5.7  # Image used
    volumes:  # Volumes for the service
      - db_data:/var/lib/mysql
    restart: always  # Restart policy
    environment:  # Environment variables
      MYSQL_ROOT_PASSWORD: wordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: ash
      MYSQL_PASSWORD: secret

  wordpress:  # Service name for WordPress
    depends_on:  # WordPress depends on MySQL
      - db
    image: wordpress:latest  # WordPress image
    ports:  # Port mapping
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: ash
      WORDPRESS_DB_PASSWORD: secret
      WORDPRESS_DB_NAME: wordpress

volumes:  # Volumes used by services
  db_data: {}
```

## Running Docker Compose
- To run the services defined in **docker-compose.yml**, use the following command: 
  ```bash
  docker-compose up
  ```
- To run Docker Compose in the **background**, use: 
  ```bash
  docker-compose up -d
  ```
- This will execute the instructions in the file, creating and starting the containers for us.
- We can stop the Compose services using `Ctrl + C`.

## Stopping Docker Compose
- To stop and remove containers, networks, and volumes defined in the file, run:
  ```bash
  docker-compose down
  ```
- To remove volumes as well, add the `--volumes` flag:
  ```bash
  docker-compose down --volumes
  ```

## Environment Variables in Compose
- We can use environment variables within Compose by defining an **env_file**.
- The syntax for calling environment variables is `${VARIABLE_NAME}`.
- This is useful for sensitive data like passwords.

```bash
mkdir variables
cd variables
touch docker-compose.yml
mkdir config
cd config
touch db.env wp.env
```

#### `wp.env` for WordPress environment variables:

```env
WORDPRESS_DB_HOST=db:3306
WORDPRESS_DB_USER=ash
WORDPRESS_DB_PASSWORD=secret
WORDPRESS_DB_NAME=wordpress
```

#### `db.env` for MySQL environment variables:

```env
MYSQL_ROOT_PASSWORD=wordpress
MYSQL_DATABASE=wordpress
MYSQL_USER=ash
MYSQL_PASSWORD=secret
```

### Docker Compose file with environment variables:

```yaml
version: '3.3'

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    env_file:  # Environment variables file
      - ./config/db.env

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    env_file:
      - ./config/wp.env

volumes:
  db_data: {}
```

To start the Compose setup:
```bash
docker-compose up -d
```

## Networks in Docker Compose
- Docker Compose creates a **default bridge network** between the containers.
- However, you can isolate networks using the `networks` key, allowing you to connect only the desired containers.
- Different **network drivers** can also be defined.

### Example with networks:

```yaml
version: '3.3'

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    env_file:
      - ./config/db.env
    networks:  # Using a custom network
      - backend

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    env_file:
      - ./config/wp.env
    networks:
      - backend

volumes:
  db_data: {}

networks:  # Defining a backend network
  backend:
    driver: bridge
```

You can inspect the networks using the following commands:
```bash
docker network ls
docker network inspect networks_backend
```

## Integrating Projects into Docker Compose
Now let's add the Flask project from the previous section into the Docker Compose setup, along with MySQL as the database.
If you didn't follow the previous section, you can find the source code [here](https://github.com/Victor-Palha/Guia-completo-de-Docker/tree/main/Compose) or read the last section for more details about the Flask project [here](https://victor-palha.github.io/posts/Docker-Guide/)

### Flask app (`app.py`):

```python
import flask
from flask import request, jsonify
import requests
from flask_mysqldb import MySQL 

app = flask.Flask(__name__)
app.config["DEBUG"] = True

app.config['MYSQL_HOST'] = 'db'  # MySQL service in Compose
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
  cur.execute("""INSERT INTO users(name) VALUES(%s)""", (username,))
  mysql.connection.commit()
  cur.close()

  return username

@app.route("/listhost", methods=['GET'])
def listhost():
  cur = mysql.connection.cursor()
  cur.execute("""SELECT * FROM users""")
  rv = cur.fetchall()
  return jsonify(rv)

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True, port=5000)
```

### Docker Compose for Flask and MySQL:

```yaml
version: '3.3'

services:
  db:
    image: mysqlcompose
    restart: always
    env_file:
      - ./config/db.env
    ports:
      - "3306:3306"
    networks:
      - dockercompose
  
  backend:
    depends_on:
      - db
    image: flaskserver
    ports:
      - "5000:5000"
    restart: always
    networks:
      - dockercompose

networks:
  dockercompose:
    driver: bridge
```

To start the setup:
```bash
docker-compose up -d
```

## Build in Docker Compose
- We can build Docker images **during the Compose** process by using the `build` key in the compose file.

```yaml
version: '3.3'

services:
  db:
    build: ./mysql-server  # Build from Dockerfile
    restart: always
    env_file:
      - ./config/db.env
    ports:
      - "3306:3306"
    networks:
      - dockercompose
  
  backend:
    depends_on:
      - db
    build: ./flask-server  # Build from Dockerfile
    ports:
      - "5000:5000"
    restart: always
    networks:
      - dockercompose

networks:
  dockercompose:
    driver: bridge
```

## Bind Mount in Docker Compose
- A **bind mount** ensures real-time updates to files in the container by syncing with the local system. It is useful during development.

```yaml
version: '3.3'

services:
  db:
    build: ./mysql-server
    restart: always
    env_file:
      - ./config/db.env
    ports:
      - "3306:3306"
    networks:
      - dockercompose
  
  backend:
    depends_on:
      - db
    build: ./flask-server
    ports:
      - "5000:5000"
    restart: always
    volumes:  # Sync local files with container files
      - /path/to/local/project:/app
    networks:
      - dockercompose

networks:
  dockercompose:
    driver: bridge
```

## Verifying Docker Compose
- You can check the running containers and their statuses with the command:
  ```bash
  docker-compose ps
  ```

# Conclusion on Docker Compose

Docker Compose is an essential tool for managing multiple containers in projects that require orchestration of various services. With it, you can define the entire application environment in a single YAML file, facilitating automation, versioning, and replication of complex environments. Compose allows for easy creation, configuration, and management of containers that work together, which is especially useful in applications with multiple dependencies.

## Key Points of Docker Compose:

1. **Definition and Execution of Multiple Containers**:
   - Enables orchestration of several images and services with a simple `docker-compose.yml` file.

2. **Ease of Use**:
   - Simple commands like `docker-compose up` and `docker-compose down` to start and stop applications.

3. **Automation**:
   - Eliminates the need to manually run multiple `docker build` and `docker run` commands, automating the process.

4. **Networks and Volumes**:
   - Support for configuring networks and volumes, allowing for efficient communication between containers and data persistence.

5. **Environment Variables**:
   - Use of `.env` files to define sensitive variables such as passwords and configurations.

6. **Scalability**:
   - Ideal for both development and production environments, allowing for scalable services with flexible configurations.

7. **Automatic Builds**:
   - Integration with Dockerfile for automatic builds, removing the need to manually create images.

8. **Service Isolation**:
   - Ability to isolate networks for containers, ensuring greater security and control among different components.

Docker Compose is thus a crucial tool for developers and system administrators who need to manage complex applications efficiently, simply, and at scale.