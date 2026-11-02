# Docker Practical on AWS EC2
## Complete Practical Notes – From EC2 Setup to Java Application Containerization

# 1. Introduction to Docker

### What is Docker?

Docker is an open-source containerization platform used to build, package, and run applications inside containers.

It packages applications along with their dependencies so that they can run consistently in different environments.

### Why do we use Docker?

- To run applications in isolated environments.
- To avoid dependency-related issues.
- To simplify application deployment.
- To make applications portable.
- To run multiple applications independently.

### Important Docker Components

| Component | Description |
|---|---|
| Docker Client | Accepts commands from users. |
| Docker Daemon | Manages Docker images and containers. |
| Docker Image | Template used to create containers. |
| Docker Container | Isolated environment in which an application runs. |
| Docker Hub | Online platform for storing and downloading images. |
| Dockerfile | File containing instructions to build an image. |

### Docker Workflow

Dockerfile → Docker Image → Docker Container

---

# PART 1: AWS EC2 INSTANCE CREATION

## 2. What is Amazon EC2?

Amazon EC2 (Elastic Compute Cloud) is an AWS service that provides virtual servers in the cloud.

We use EC2 to create virtual machines where we can install software, host applications, and perform DevOps practicals.

### Step-by-step EC2 creation

**Step 1: Login to AWS**

1. Open https://console.aws.amazon.com/
2. Login to your AWS account.
3. Search for EC2.
4. Open the EC2 Dashboard.

**Step 2: Launch an instance**

1. Click Launch Instance.
2. Enter the instance name: `Docker-S`
3. Select Ubuntu Server 24.04 LTS.
4. Select an eligible instance type.

**Step 3: Create a Key Pair**

1. Click Create new key pair.
2. Key pair name: `Docker-key`
3. Key pair type: RSA.
4. Private key format: `.pem`
5. Click Create key pair.
6. Download the key file.

**Step 4: Configure Network Settings**

1. Enable Allow SSH traffic.
2. Select My IP.
3. SSH uses port 22.

**Step 5: Configure Storage**

Select the required storage.

**Step 6: Launch Instance**

Click Launch Instance.

Wait until:
- Instance state: Running
- Status checks: 2/2 checks passed

Your Ubuntu EC2 instance is ready.

---

# PART 2: CONNECT EC2 USING MOBAXTERM

## 3. What is MobaXterm?

MobaXterm is a remote connection tool used to access Linux servers through SSH.

It also provides an SFTP browser to transfer files between your laptop and the remote server.

### Steps to connect EC2

**Step 1:** Open MobaXterm.

**Step 2:** Click Session → SSH.

**Step 3:** Enter the following details:

| Field | Value |
|---|---|
| Remote host | EC2 Public IPv4 address |
| Username | ubuntu |
| Port | 22 |

**Step 4:** Open Advanced SSH settings.

- Enable Use private key.
- Select your `Docker-key.pem` file.

**Step 5:** Click OK.

After successful connection, you will see a terminal similar to:

```bash
ubuntu@ip-172-31-13-60:~$
```

### Basic Linux commands

**1. Check current directory**

```bash
pwd
```

Output:

```text
/home/ubuntu
```

**2. List files and folders**

```bash
ls
```

**3. Check current username**

```bash
whoami
```

Output:

```text
ubuntu
```

---

# PART 3: DOCKER INSTALLATION ON UBUNTU EC2

## 4. Update Ubuntu packages

Before installing Docker, update the package information.

Command:

```bash
sudo apt-get update
```

What it does:
- `sudo` – Executes the command with administrator privileges.
- `apt-get` – Ubuntu package management tool.
- `update` – Updates available package information.

Expected output:

```text
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
```

## 5. Install Docker

Command:

```bash
sudo apt-get install docker.io
```

This installs Docker Engine on Ubuntu.

## 6. Check Docker service status

Command:

```bash
sudo systemctl status docker
```

Expected status:

```text
Active: active (running)
```

This confirms that the Docker service is running.

## 7. Start and enable Docker

Start Docker:

```bash
sudo systemctl start docker
```

Enable Docker at system startup:

```bash
sudo systemctl enable docker
```

## 8. Check Docker commands

Command:

```bash
docker
```

This displays Docker help and available commands.

## 9. Check running containers

Command:

```bash
docker ps
```

This displays currently running containers.

To display all containers:

```bash
docker ps -a
```

## 10. Give Docker permission to Ubuntu user

Initially, Docker commands may require administrator privileges.

First, check the current username:

```bash
whoami
```

Output:

```text
ubuntu
```

Add the current user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

### Explanation

| Part | Meaning |
|---|---|
| sudo | Administrator privileges |
| usermod | Modify an existing user |
| -a | Append the group |
| -G | Specify supplementary groups |
| docker | Docker group |
| $USER | Current logged-in user |

Refresh group membership:

```bash
newgrp docker
```

Now execute:

```bash
docker ps
```

Docker commands can now be executed without typing `sudo` each time.

---

# PART 4: DOCKER HUB LOGIN

## 11. What is Docker Hub?

Docker Hub is an online container registry used to store, download, and share Docker images.

Website: https://hub.docker.com/

### Login to Docker Hub

Command:

```bash
docker login -u devdhareakshay2104
```

Enter your Docker Hub password or Personal Access Token when prompted.

Expected output:

```text
Login Succeeded
```

### Login using Personal Access Token (PAT)

Steps:

1. Open Docker Hub.
2. Login to your account.
3. Go to Account Settings.
4. Open Personal Access Tokens.
5. Click Generate New Token.
6. Enter token name: `Docker-S`.
7. Select the required permissions.
8. Generate the token.
9. Copy and securely save the token.

Execute:

```bash
docker login -u devdhareakshay2104
```

Enter the generated token when prompted for the password.

---

# PART 5: HELLO-WORLD DOCKER IMAGE

## 12. Download Hello World image

Command:

```bash
docker pull hello-world
```

This downloads the Hello World image from Docker Hub.

Expected output:

```text
Using default tag: latest
Status: Downloaded newer image for hello-world:latest
```

## 13. Display Docker images

Command:

```bash
docker images
```

Example output:

```text
REPOSITORY     TAG       IMAGE ID       CREATED       SIZE
hello-world    latest    xxxxxxxx       ...           ...
```

This displays all locally available Docker images.

## 14. Run Hello World container

Command:

```bash
docker run hello-world
```

Expected output:

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### What happens when we execute docker run?

1. Docker checks whether the image is available locally.
2. If unavailable, Docker downloads the image.
3. Docker creates a container.
4. The container executes the application.
5. Output appears in the terminal.

---

# PART 6: MYSQL CONTAINER PRACTICAL

## 15. Download MySQL image

Command:

```bash
docker pull mysql
```

This downloads the MySQL image from Docker Hub.

Check downloaded images:

```bash
docker images
```

## 16. Run MySQL container

Command:

```bash
docker run -e MYSQL_ROOT_PASSWORD=root mysql
```

### Explanation

| Part | Meaning |
|---|---|
| docker run | Creates and starts a container |
| -e | Sets an environment variable |
| MYSQL_ROOT_PASSWORD | MySQL root password variable |
| root | Password value used in this practical |
| mysql | MySQL image |

### Run MySQL in the background

Command:

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=root mysql
```

Here, `-d` means detached mode.

It runs the MySQL container in the background, allowing us to execute other commands in the same terminal.

### Run with a specific container name

Command:

```bash
docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD=root mysql
```

Explanation:

- `-d` – Runs in the background.
- `--name` – Assigns a container name.
- `my-mysql` – Container name.
- `-e` – Sets an environment variable.
- `mysql` – Image name.

Expected output:

```text
<container-id>
```

## 17. Check running MySQL container

Command:

```bash
docker ps
```

Example output:

```text
CONTAINER ID   IMAGE   COMMAND   STATUS
xxxxxxxx       mysql   ...       Up ...
```

## 18. Stop MySQL container

Command:

```bash
docker stop my-mysql
```

Expected output:

```text
my-mysql
```

## 19. Remove MySQL container

Command:

```bash
docker rm my-mysql
```

This removes the stopped container.

To check all containers:

```bash
docker ps -a
```

### Important difference

- `docker stop` – Stops a container.
- `docker start` – Starts an existing stopped container.
- `docker rm` – Removes a container.
- `docker images` – Displays images.

---

# PART 7: JAVA PROJECT USING DOCKER

## 20. GitHub repository

Project name: Simple Java Docker

GitHub repository:

https://github.com/LondheShubham153/simple-java-docker.git

### What is Git Clone?

Git clone is a command used to copy an existing remote GitHub repository to a local machine or server.

Clone command:

```bash
git clone https://github.com/LondheShubham153/simple-java-docker.git
```

## 21. Navigate to the project directory

Create a projects directory:

```bash
mkdir -p ~/projects
```

Navigate to it:

```bash
cd ~/projects
```

Display available folders:

```bash
ls
```

Open the project:

```bash
cd simple-java-docker
```

Check project files:

```bash
ls
```

Project structure:

```text
simple-java-docker/
│
├── Dockerfile
│
└── src/
    └── Main.java
```

---

# PART 8: CREATE DOCKERFILE

## 22. What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

### Important Dockerfile instructions

| Instruction | Purpose |
|---|---|
| FROM | Specifies the base image. |
| WORKDIR | Sets the working directory. |
| COPY | Copies files into the image. |
| RUN | Executes commands during image building. |
| CMD | Specifies the default command when the container starts. |

## 23. Create or edit Dockerfile

Command:

```bash
vim Dockerfile
```

Dockerfile content:

```dockerfile
# Pull a base image which gives all required tools and libraries
FROM eclipse-temurin:17-jdk-alpine

# Create a folder where the app code will be stored
WORKDIR /app

# Copy the source code from your host machine to your container
COPY src/Main.java /app/Main.java

# Compile the application code
RUN javac Main.java

# Run the application
CMD ["java", "Main"]
```

### Explanation

**FROM**

```dockerfile
FROM eclipse-temurin:17-jdk-alpine
```

Downloads the Java 17 JDK base image.

**WORKDIR**

```dockerfile
WORKDIR /app
```

Creates and sets `/app` as the working directory inside the image.

**COPY**

```dockerfile
COPY src/Main.java /app/Main.java
```

Copies the Java source code from the project directory into the container image.

**RUN**

```dockerfile
RUN javac Main.java
```

Compiles the Java application.

**CMD**

```dockerfile
CMD ["java", "Main"]
```

Executes the Java application when the container starts.

## 24. Display Dockerfile

Command:

```bash
cat Dockerfile
```

This displays the Dockerfile content in the terminal.

---

# PART 9: BUILD JAVA DOCKER IMAGE

## 25. Build the image

Command:

```bash
docker build -t java-app .
```

### Explanation

| Part | Meaning |
|---|---|
| docker build | Builds a Docker image |
| -t | Assigns a name to the image |
| java-app | Image name |
| . | Current directory as build context |

Expected output:

```text
Step 1/5 : FROM eclipse-temurin:17-jdk-alpine
Step 2/5 : WORKDIR /app
Step 3/5 : COPY src/Main.java /app/Main.java
Step 4/5 : RUN javac Main.java
Step 5/5 : CMD ["java", "Main"]

Successfully built xxxxxxxxxxxx
Successfully tagged java-app:latest
```

The image ID will be generated by Docker.

## 26. Check Docker images

Command:

```bash
docker images
```

Example output:

```text
REPOSITORY       TAG              IMAGE ID
java-app         latest           xxxxxxxxx
eclipse-temurin  17-jdk-alpine    xxxxxxxxx
```

This confirms that your Java application image has been created.

---

# PART 10: RUN JAVA APPLICATION CONTAINER

## 27. Run Java application

Command:

```bash
docker run java-app
```

This creates and starts a container using the `java-app` image.

The Java application executes inside the container, and its output appears in the terminal.

### Run with a container name

```bash
docker run --name java-container java-app
```

### Check containers

```bash
docker ps
```

To display all containers:

```bash
docker ps -a
```

---

# PRACTICAL COMMANDS COMPLETED

## 28. Complete command reference

### EC2 and Linux

```bash
pwd
ls
whoami
```

### Docker installation

```bash
sudo apt-get update
sudo apt-get install docker.io
sudo systemctl status docker
sudo systemctl start docker
sudo systemctl enable docker
```

### Docker permissions

```bash
sudo usermod -aG docker $USER
newgrp docker
docker ps
```

### Docker Hub

```bash
docker login -u devdhareakshay2104
```

### Hello World

```bash
docker pull hello-world
docker images
docker run hello-world
```

### MySQL

```bash
docker pull mysql
docker images
docker run -e MYSQL_ROOT_PASSWORD=root mysql
docker run -d -e MYSQL_ROOT_PASSWORD=root mysql
docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD=root mysql
docker ps
docker stop my-mysql
docker rm my-mysql
```

### Java Docker project

```bash
mkdir -p ~/projects
cd ~/projects

git clone https://github.com/LondheShubham153/simple-java-docker.git

cd simple-java-docker

ls

vim Dockerfile

cat Dockerfile

docker build -t java-app .

docker images

docker run java-app
```

---

# PRACTICAL LEARNING OUTCOME

By completing this practical, I learned:

1. How to create an Ubuntu EC2 instance on AWS.
2. How to connect an EC2 instance using MobaXterm.
3. How to install Docker on Ubuntu.
4. How to configure Docker user permissions.
5. How to login to Docker Hub.
6. How to download Docker images.
7. How to run a Hello World container.
8. How to download and run a MySQL container.
9. How to execute containers in the background.
10. How to stop and remove containers.
11. How to clone a Java project from GitHub.
12. How to create a Dockerfile.
13. How to build a Java Docker image.
14. How to run a Java application inside a Docker container.

**End of Practical – Docker Installation and Java Application Containerization**

# Docker Networking Practical on AWS EC2
## Two-Tier Flask Application with MySQL

# 1. Introduction to Docker Networking

### What is Docker Networking?

Docker Networking is a feature of Docker that allows containers to communicate with each other, the host machine, and external networks.

It helps different containers exchange information and work together as part of an application.

### Why do we use Docker Networking?

- To allow containers to communicate with each other.
- To connect application containers with database containers.
- To allow containers to access external networks.
- To provide network isolation.
- To connect multiple application services.

### Types of Docker Networks

| Network | Description |
|---|---|
| Bridge | Allows containers on the same Docker host to communicate. |
| Host | Uses the host machine's network namespace. |
| None | Provides network isolation. |
| Overlay | Connects containers running on multiple Docker hosts. |
| Macvlan | Gives containers their own MAC addresses. |

### Docker Networking Architecture

Docker Network → Flask Container → MySQL Container

Both containers communicate through a common Docker network.

---

# PART 1: DOCKER NETWORKING COMMANDS

## 2. Check Available Docker Networks

Command:

```bash
docker network ls
```

This command displays all available Docker networks.

Expected output:

```text
NETWORK ID     NAME      DRIVER
xxxxxxxx       bridge    bridge
xxxxxxxx       host      host
xxxxxxxx       none      null
```

### Explanation

- `docker network` – Docker networking command.
- `ls` – Lists available networks.

## 3. Create Docker Network

Command:

```bash
docker network create mynetwork -d bridge
```

### Explanation

| Part | Meaning |
|---|---|
| docker network create | Creates a Docker network. |
| mynetwork | Network name. |
| -d | Specifies the network driver. |
| bridge | Bridge network driver. |

Expected output:

```text
xxxxxxxxxxxxxxxxxxxxxxxx
```

Check the network:

```bash
docker network ls
```

Now you will see `mynetwork` in the network list.

### What is Bridge Networking?

Bridge networking allows containers running on the same Docker host to communicate with each other.

---

# PART 2: TWO-TIER FLASK APPLICATION

## 4. What is a Two-Tier Application?

A two-tier application is an application architecture that consists of two main layers.

**1. Application Tier**

- Handles user requests.
- Processes application logic.
- Communicates with the database.
- In this practical, we use Flask.

**2. Database Tier**

- Stores application data.
- Manages database records.
- Handles data retrieval.
- In this practical, we use MySQL.

### Technologies Used

| Technology | Purpose |
|---|---|
| AWS EC2 | Cloud server |
| Ubuntu | Operating system |
| MobaXterm | SSH connection |
| Docker | Containerization |
| Docker Network | Container communication |
| Flask | Backend application |
| MySQL | Database |
| GitHub | Source code repository |

### Project Architecture

```text
          USER
            |
            |
       WEB BROWSER
            |
            |
       AWS EC2 SERVER
            |
            |
      DOCKER NETWORK
         two-tier
            |
       --------------
       |            |
       |            |
  FLASK APP       MYSQL
  CONTAINER      CONTAINER
       |            |
       |            |
  APPLICATION    DATABASE
     TIER          TIER
```

### Why do we need Docker Networking in this project?

Here, Flask and MySQL are running inside separate Docker containers.

The Flask application needs to communicate with MySQL to store and retrieve information.

Therefore, we create a common Docker network named `two-tier` and connect both containers to it.

---

# PART 3: CLONE GITHUB REPOSITORY

## 5. GitHub Repository

Project name: Two-Tier Flask Application

GitHub Repository:

https://github.com/LondheShubham153/two-tier-flask-app.git

### What is Git Clone?

Git clone is a command used to download an existing GitHub repository to our server.

### Step 1: Navigate to Projects Directory

```bash
cd ~/projects
```

### Step 2: Clone Repository

```bash
git clone https://github.com/LondheShubham153/two-tier-flask-app.git
```

Expected output:

```text
Cloning into 'two-tier-flask-app'...
remote: Enumerating objects...
Receiving objects...
```

### Step 3: Navigate to Project

```bash
cd two-tier-flask-app
```

### Step 4: Display Project Files

```bash
ls
```

### Step 5: Display Application Code

```bash
cat app.py
```

This displays the Flask application source code.

---

# PART 4: CREATE DOCKERFILE AND BUILD IMAGE

## 6. What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

It specifies the base image, working directory, application files, dependencies, and commands required to run an application.

### Step 1: Open Dockerfile

```bash
vim Dockerfile
```

### Step 2: Add Dockerfile Instructions

Use the Dockerfile provided in the GitHub repository.

### Step 3: Display Dockerfile

```bash
cat Dockerfile
```

### Step 4: Build Flask Backend Image

Command:

```bash
docker build -t two-tier-backend .
```

### Explanation

| Part | Meaning |
|---|---|
| docker build | Builds Docker image. |
| -t | Assigns image name. |
| two-tier-backend | Image name. |
| . | Current directory. |

Expected output:

```text
Successfully built xxxxxxxxx
Successfully tagged two-tier-backend:latest
```

### Step 5: Check Docker Images

```bash
docker images
```

Example output:

```text
REPOSITORY          TAG       IMAGE ID
two-tier-backend    latest    xxxxxxxxx
```

---

# PART 5: CREATE TWO-TIER NETWORK

## 7. Create Docker Network

Command:

```bash
docker network create two-tier -d bridge
```

### Explanation

- `docker network create` – Creates a new network.
- `two-tier` – Network name.
- `-d` – Specifies the driver.
- `bridge` – Bridge network.

Expected output:

```text
xxxxxxxxxxxxxxxxxxxxxxxx
```

### Step 2: Check Docker Networks

```bash
docker network ls
```

You should see:

```text
NETWORK ID     NAME       DRIVER
xxxxxxxx       bridge     bridge
xxxxxxxx       host       host
xxxxxxxx       none       null
xxxxxxxx       two-tier   bridge
```

### Step 3: Inspect Docker Network

```bash
docker network inspect two-tier
```

This displays network information, including connected containers, subnet, gateway, and network driver.

---

# PART 6: RUN MYSQL CONTAINER

## 8. Create MySQL Container

MySQL is used as the database tier in this project.

Command:

```bash
docker run -d --name mysql --network two-tier -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
```

### Explanation

| Part | Meaning |
|---|---|
| docker run | Creates and starts a container. |
| -d | Runs container in background. |
| --name mysql | Assigns container name. |
| --network two-tier | Connects container to Docker network. |
| -e | Sets environment variables. |
| MYSQL_ROOT_PASSWORD=root | Sets MySQL root password. |
| MYSQL_DATABASE=devops | Creates database named devops. |
| mysql | MySQL image. |

Expected output:

```text
xxxxxxxxxxxxxxxxxxxxxxxx
```

### Step 2: Check Running Containers

```bash
docker ps
```

You should see the MySQL container with status `Up`.

### Important Concept: MYSQL_HOST

In our Flask application, we use:

```bash
MYSQL_HOST=mysql
```

Here, `mysql` is the name of the MySQL container.

Docker networking allows the Flask container to find the MySQL container using its container name.

---

# PART 7: RUN FLASK BACKEND CONTAINER

## 9. Run Flask Application

Now we run the Flask backend container and connect it to the same Docker network.

Command:

```bash
docker run -d -p 5000:5000 --network two-tier -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=root -e MYSQL_DB=devops two-tier-backend:latest
```

### Explanation

| Part | Meaning |
|---|---|
| docker run | Creates and starts a container. |
| -d | Runs in background. |
| -p 5000:5000 | Maps host port 5000 to container port 5000. |
| --network two-tier | Connects to Docker network. |
| MYSQL_HOST=mysql | MySQL container hostname. |
| MYSQL_USER=root | MySQL username. |
| MYSQL_PASSWORD=root | MySQL password. |
| MYSQL_DB=devops | Database name. |
| two-tier-backend:latest | Flask backend image. |

### What is Port Mapping?

```bash
-p 5000:5000
```

- First 5000 – EC2 host port.
- Second 5000 – Container port.

It allows users to access the Flask application through the EC2 public IP.

### Step 2: Check Running Containers

```bash
docker ps
```

You should see two containers:

1. MySQL container.
2. Flask backend container.

---

# PART 8: VERIFY DOCKER NETWORK

## 10. Check Network Connections

Command:

```bash
docker network inspect two-tier
```

This displays all containers connected to the `two-tier` network.

You should see both:

- MySQL container.
- Flask backend container.

### Why is this important?

Both containers must be connected to the same network so that the Flask application can communicate with the MySQL database using the hostname `mysql`.

## 11. Check Docker Container Logs

Command:

```bash
docker ps
```

Copy the container ID or name.

To check MySQL logs:

```bash
docker logs mysql
```

To check backend logs:

```bash
docker logs <container_id>
```

Logs help us identify application and database connection problems.

---

# PART 9: CONNECT TO MYSQL DATABASE

## 12. Access MySQL Container

Command:

```bash
docker exec -it mysql bash
```

### Explanation

| Part | Meaning |
|---|---|
| docker exec | Executes a command inside a running container. |
| -it | Interactive terminal. |
| mysql | Container name. |
| bash | Opens Bash shell. |

## 13. Login to MySQL

Command:

```bash
mysql -u root -p
```

Enter password:

```text
root
```

Expected output:

```text
Welcome to the MySQL monitor.
mysql>
```

## 14. Check Database

Display databases:

```sql
SHOW DATABASES;
```

Select the database:

```sql
USE devops;
```

Expected output:

```text
Database changed
```

Display tables:

```sql
SHOW TABLES;
```

Display records:

```sql
SELECT * FROM messages;
```

This displays the records stored in the messages table, if that table exists.

### Exit MySQL

```sql
EXIT;
```

Exit the container:

```bash
exit
```

---

# PART 10: CONFIGURE AWS SECURITY GROUP

## 15. Allow Port 5000

To access the Flask application from Chrome, we need to allow inbound traffic on port 5000.

### Steps

1. Open AWS Console.
2. Go to EC2.
3. Select your instance.
4. Open Security.
5. Click the Security Group.
6. Select Edit inbound rules.
7. Click Add rule.

Configure:

| Setting | Value |
|---|---|
| Type | Custom TCP |
| Protocol | TCP |
| Port range | 5000 |
| Source | My IP |

8. Click Save rules.

---

# PART 11: ACCESS APPLICATION THROUGH CHROME

## 16. Open Flask Application

Copy the Public IPv4 address of your EC2 instance.

Open Chrome and enter:

```text
http://YOUR_EC2_PUBLIC_IP:5000
```

Example:

```text
http://13.201.99.126:5000
```

Your Flask application should open in the browser.

### How does the application work?

1. User opens the application in Chrome.
2. Request reaches the EC2 instance on port 5000.
3. Docker forwards traffic to the Flask container.
4. Flask processes the request.
5. Flask communicates with MySQL through the `two-tier` network.
6. MySQL stores or retrieves the requested data.
7. Flask returns the response to the browser.

---

# PART 12: COMPLETE COMMAND REFERENCE

## 17. All Practical Commands

### Docker Networking

```bash
cd ~/projects

docker network ls

docker network create mynetwork -d bridge

docker network ls

docker network inspect mynetwork
```

### GitHub Repository

```bash
git clone https://github.com/LondheShubham153/two-tier-flask-app.git

cd two-tier-flask-app

ls

cat app.py
```

### Build Backend Image

```bash
vim Dockerfile

cat Dockerfile

docker build -t two-tier-backend .

docker images
```

### Create Two-Tier Network

```bash
docker network create two-tier -d bridge

docker network ls

docker network inspect two-tier
```

### Run MySQL Container

```bash
docker run -d --name mysql --network two-tier -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
```

### Run Flask Backend

```bash
docker run -d -p 5000:5000 --network two-tier -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=root -e MYSQL_DB=devops two-tier-backend:latest
```

### Check Containers

```bash
docker ps

docker ps -a

docker images

docker network ls

docker network inspect two-tier
```

### Check Logs

```bash
docker logs mysql

docker logs <container_id>
```

### Connect to MySQL

```bash
docker exec -it mysql bash

mysql -u root -p
```

### MySQL Commands

```sql
SHOW DATABASES;

USE devops;

SHOW TABLES;

SELECT * FROM messages;

EXIT;
```

### Exit Container

```bash
exit
```

---

# 18. PRACTICAL LEARNING OUTCOME

By completing this practical, I learned:

1. What Docker Networking is.
2. Different types of Docker networks.
3. How to create a bridge network.
4. How to clone a Flask project from GitHub.
5. How to build a Flask Docker image.
6. How to create a two-tier Docker network.
7. How to run a MySQL container.
8. How to run a Flask backend container.
9. How to connect two containers using Docker networking.
10. How to use environment variables for database connectivity.
11. How to inspect Docker network connections.
12. How to access MySQL inside a container.
13. How to execute SQL commands.
14. How to configure AWS Security Group port 5000.
15. How to access a Flask application through a browser.

**End of Practical – Docker Networking with Two-Tier Flask Application**


# Docker Volumes and Storage Practical on AWS EC2
## Complete Practical Notes – Persistent Data Storage in Docker

# 1. Introduction to Docker Volumes

### What are Docker Volumes?

Docker Volumes are used to store data generated by Docker containers outside the container's writable layer.

They help preserve data even when a container is stopped, deleted, or recreated.

### Why do we use Docker Volumes?

- To store data permanently.
- To prevent data loss when containers are deleted.
- To maintain database records.
- To share data between containers.
- To separate application data from container lifecycle.

### What is Persistent Storage?

Persistent storage means storing data so that it remains available even after a container is stopped or removed.

### Important Docker Storage Concepts

| Component | Description |
|---|---|
| Docker Volume | Storage managed by Docker. |
| Container | Runs an application or database. |
| Volume Mount | Connects a volume to a container directory. |
| Bind Mount | Connects a host directory to a container directory. |
| `/var/lib/mysql` | Default MySQL data directory inside the container. |

### Docker Storage Architecture

```text
       AWS EC2 SERVER
             |
          DOCKER
             |
       MYSQL CONTAINER
             |
       /var/lib/mysql
             |
       DOCKER VOLUME
             |
         MYSQL DATA
```

---

# PART 1: CHECK EXISTING DOCKER VOLUMES

## 2. Navigate to Project Directory

Command:

```bash
cd ~/projects/two-tier-flask-app
```

## 3. Display Docker Volumes

Command:

```bash
docker volume ls
```

This command displays all Docker volumes available on the system.

Example output:

```text
DRIVER    VOLUME NAME
local     mysql-data
```

## 4. Create Docker Volume

Command:

```bash
docker volume create mysql-data
```

### Explanation

| Part | Meaning |
|---|---|
| docker volume | Docker volume management command. |
| create | Creates a new volume. |
| mysql-data | Name of the volume. |

Expected output:

```text
mysql-data
```

## 5. Inspect Docker Volume

Command:

```bash
docker inspect mysql-data
```

This displays detailed information about the volume.

Example output:

```json
[
    {
        "Name": "mysql-data",
        "Driver": "local",
        "Mountpoint": "/var/lib/docker/volumes/mysql-data/_data"
    }
]
```

### What is Mountpoint?

Mountpoint is the physical location where Docker stores the volume data on the EC2 server.

Example:

```text
/var/lib/docker/volumes/mysql-data/_data
```

## 6. Check Running Containers

Command:

```bash
docker ps
```

This displays all currently running containers.

---

# PART 2: RUN MYSQL CONTAINER WITH DOCKER VOLUME

## 7. What is Volume Mounting?

Volume mounting is the process of connecting Docker storage to a specific directory inside a container.

In this practical, we connect the Docker volume `mysql-data` to the MySQL data directory `/var/lib/mysql`.

## 8. Run MySQL Container with Volume

Command:

```bash
docker run -d --name mysql --network two-tier -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
```

### Explanation

| Part | Meaning |
|---|---|
| docker run | Creates and starts a container. |
| -d | Runs container in background. |
| --name mysql | Assigns container name. |
| --network two-tier | Connects container to Docker network. |
| -v | Mounts a volume. |
| mysql-data | Docker volume name. |
| /var/lib/mysql | MySQL data directory inside container. |
| MYSQL_ROOT_PASSWORD=root | Sets MySQL root password. |
| MYSQL_DATABASE=devops | Creates database named devops. |
| mysql | MySQL image. |

### Important Command Explanation

```bash
-v mysql-data:/var/lib/mysql
```

- `mysql-data` – Volume name.
- `/var/lib/mysql` – Container storage location.

This ensures that MySQL data is stored in the Docker volume instead of only inside the container.

## 9. Check Running Containers

Command:

```bash
docker ps
```

Example output:

```text
CONTAINER ID   IMAGE   STATUS
xxxxxxxx       mysql   Up ...
```

---

# PART 3: UNDERSTAND DATA PERSISTENCE

## 10. What Happens When We Delete a Container?

Normally, data stored only inside a container can be lost when the container is deleted.

However, when we use Docker volumes, the data remains stored separately.

### Step 1: Restart MySQL Container

Command:

```bash
docker restart mysql
```

This restarts the existing MySQL container.

### Step 2: Check Containers

```bash
docker ps
```

### Step 3: Stop MySQL Container

```bash
docker stop mysql
```

### Step 4: Remove MySQL Container

```bash
docker rm mysql
```

### Step 5: Check Docker Volumes

```bash
docker volume ls
```

You will still see:

```text
DRIVER    VOLUME NAME
local     mysql-data
```

**Important:** Removing the container does not automatically delete the named volume.

---

# PART 4: CHECK DOCKER VOLUME DATA

## 11. Inspect Volume Location

Command:

```bash
docker inspect mysql-data
```

Find the Mountpoint:

```text
/var/lib/docker/volumes/mysql-data/_data
```

## 12. Access Docker Volume Directory

Switch to administrator privileges:

```bash
sudo su
```

Navigate to the volume directory:

```bash
cd /var/lib/docker/volumes/mysql-data/_data
```

Display files:

```bash
ls
```

This allows you to inspect the files stored in the MySQL volume.

**Note:** MySQL should be stopped before manually inspecting or working with its data files. Do not manually edit or delete MySQL database files.

---

# PART 5: CREATE HOST DIRECTORY FOR BIND MOUNT

## 13. What is a Bind Mount?

A Bind Mount connects a specific directory from the host machine to a directory inside a Docker container.

Unlike named volumes, bind mounts use a host directory path that we specify.

### Difference Between Volume and Bind Mount

| Docker Volume | Bind Mount |
|---|---|
| Managed by Docker. | Managed using a host directory. |
| Uses a volume name. | Uses an absolute host path. |
| Example: `mysql-data` | Example: `/home/ubuntu/volume/mysql` |
| Docker manages its storage location. | User chooses the host directory. |

## 14. Create Host Directory

Return to your Ubuntu home directory:

```bash
cd /home/ubuntu
```

Create a directory:

```bash
mkdir volumes
```

Check directories:

```bash
ls
```

Navigate into volumes:

```bash
cd volumes
```

Create MySQL directory:

```bash
mkdir mysql
```

Check files:

```bash
ls
```

Check current path:

```bash
pwd
```

Expected output:

```text
/home/ubuntu/volumes
```

Navigate into MySQL directory:

```bash
cd mysql
```

Check current path:

```bash
pwd
```

Expected output:

```text
/home/ubuntu/volumes/mysql
```

Return to home directory:

```bash
cd
```

---

# PART 6: RUN MYSQL USING BIND MOUNT

## 15. Stop and Remove Existing MySQL Container

Navigate to the project directory:

```bash
cd ~/projects/two-tier-flask-app
```

Check running containers:

```bash
docker ps
```

Stop MySQL:

```bash
docker stop mysql
```

Remove MySQL container:

```bash
docker rm mysql
```

**Note:** Removing the container does not remove the previously created named volume.

## 16. Run MySQL with Bind Mount

Command:

```bash
docker run -d --name mysql --network two-tier -v /home/ubuntu/volumes/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
```

### Explanation

| Part | Meaning |
|---|---|
| -d | Runs in background. |
| --name mysql | Container name. |
| --network two-tier | Docker network. |
| -v | Creates a bind mount. |
| /home/ubuntu/volumes/mysql | Host directory. |
| /var/lib/mysql | MySQL container directory. |
| MYSQL_ROOT_PASSWORD=root | MySQL root password. |
| MYSQL_DATABASE=devops | Database name. |
| mysql | MySQL image. |

### Important Command

```bash
-v /home/ubuntu/volumes/mysql:/var/lib/mysql
```

Here:

- Left side: Directory on the EC2 Ubuntu server.
- Right side: Directory inside the MySQL container.

This means MySQL data is stored in the Ubuntu host directory.

## 17. Check Running Container

Command:

```bash
docker ps
```

## 18. Check Host Directory Data

Navigate to the directory:

```bash
cd /home/ubuntu/volumes/mysql
```

Display files:

```bash
ls
```

You should see MySQL data files and directories after MySQL initializes successfully.

---

# PART 7: VERIFY DATA PERSISTENCE

## 19. Verify MySQL Data

The purpose of this practical is to ensure that database data remains available even after removing and recreating the container.

### Step 1: Stop MySQL

```bash
docker stop mysql
```

### Step 2: Remove MySQL Container

```bash
docker rm mysql
```

### Step 3: Run MySQL Again Using the Same Bind Mount

```bash
docker run -d --name mysql --network two-tier -v /home/ubuntu/volumes/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
```

### Step 4: Check Container

```bash
docker ps
```

### Step 5: Connect to MySQL

```bash
docker exec -it mysql bash
```

Login:

```bash
mysql -u root -p
```

Enter password:

```text
root
```

### Step 6: Check Database

```sql
SHOW DATABASES;
```

Select database:

```sql
USE devops;
```

Display tables:

```sql
SHOW TABLES;
```

If the application previously created database records, verify them using the appropriate table and SELECT query.

**Result:** The data remains available because the same host directory is mounted again to `/var/lib/mysql`.

---

# PART 8: IMPORTANT DOCKER VOLUME COMMANDS

## 20. Complete Command Reference

### Docker Volume Commands

```bash
docker volume ls

docker volume create mysql-data

docker inspect mysql-data

docker volume inspect mysql-data
```

### Container Commands

```bash
docker ps

docker ps -a

docker restart mysql

docker stop mysql

docker rm mysql
```

### Named Volume

```bash
docker run -d --name mysql --network two-tier -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
```

### Access Volume Directory

```bash
sudo su

cd /var/lib/docker/volumes/mysql-data/_data

ls
```

### Create Host Directory

```bash
cd /home/ubuntu

mkdir volumes

cd volumes

mkdir mysql

ls

pwd

cd mysql

pwd
```

### Bind Mount

```bash
docker run -d --name mysql --network two-tier -v /home/ubuntu/volumes/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
```

### Check Host Data

```bash
cd /home/ubuntu/volumes/mysql

ls
```

---

# 21. IMPORTANT DEFINITIONS FOR INTERVIEW

**1. What is Docker Volume?**

Docker Volume is a storage mechanism used to persist data generated by containers independently of their lifecycle.

**2. What is Persistent Storage?**

Persistent storage is a method of storing data so that it remains available even after a container is stopped or deleted.

**3. What is Bind Mount?**

A Bind Mount connects a specific directory on the host machine to a directory inside a container.

**4. What is the difference between Docker Volume and Bind Mount?**

Docker Volumes are managed by Docker, whereas Bind Mounts use directories directly from the host machine.

**5. What is `/var/lib/mysql`?**

It is the default directory inside the MySQL container where MySQL stores its database files.

**6. Why do we use `-v` in Docker?**

The `-v` option is used to mount a Docker volume or a host directory inside a container.

**7. What happens to a Docker Volume when a container is deleted?**

A named Docker volume remains available even after the associated container is deleted, unless the volume itself is removed.

---

# 22. PRACTICAL LEARNING OUTCOME

By completing this practical, I learned:

1. What Docker Volumes are.
2. Why persistent storage is required.
3. How to list Docker volumes.
4. How to create a Docker volume.
5. How to inspect Docker volumes.
6. How to connect MySQL with Docker volumes.
7. How to restart, stop, and remove containers.
8. How to access Docker volume directories.
9. What Bind Mounts are.
10. How to create host directories.
11. How to connect host directories with containers.
12. How to preserve MySQL data after deleting and recreating containers.
13. Difference between Docker Volumes and Bind Mounts.

**End of Practical – Docker Volumes and Persistent Storage**


# Docker Compose Practical on AWS EC2

## Two-Tier Flask Application with MySQL

# 1. Introduction to Docker Compose

### What is Docker Compose?

Docker Compose is a tool used to define and manage multiple Docker containers using a single YAML configuration file.

It allows us to configure services, networks, volumes, ports, and environment variables in one file and run multiple containers together.

### Why do we use Docker Compose?

- To manage multiple containers together.
- To automate container creation and execution.
- To connect application and database containers.
- To configure networks and volumes.
- To reduce the number of Docker commands.
- To simplify application deployment.

### Important Components of Docker Compose

| Component | Purpose |
|---|---|
| Services | Defines containers. |
| Image | Specifies Docker image. |
| Build | Builds an image using Dockerfile. |
| Ports | Maps host and container ports. |
| Environment | Defines environment variables. |
| Volumes | Provides persistent storage. |
| Networks | Connects containers. |
| Depends_on | Defines service dependencies. |
| Restart | Defines container restart policy. |
| Healthcheck | Checks container health. |

---

# PART 1: DOCKER COMPOSE INSTALLATION

## 2. Navigate to Project Directory

Command:

```bash
cd ~/projects/two-tier-flask-app
```

### Explanation

- `cd` – Changes the directory.
- `projects` – Projects folder.
- `two-tier-flask-app` – Our application directory.

## 3. Install Docker Compose

Command:

```bash
sudo apt-get update
```

This command updates the Ubuntu package list.

Install Docker Compose V2:

```bash
sudo apt-get install docker-compose-v2
```

### Explanation

- `sudo` – Executes command with administrator privileges.
- `apt-get install` – Installs a package.
- `docker-compose-v2` – Docker Compose V2 package.

## 4. Verify Docker Compose Installation

Command:

```bash
docker compose version
```

Expected output:

```text
Docker Compose version v2.x.x
```

This confirms that Docker Compose is installed.

---

# PART 2: CREATE DOCKER COMPOSE FILE

## 5. What is docker-compose.yml?

`docker-compose.yml` is a YAML configuration file used to define multiple Docker services and their configurations.

In our project, we define two services:

1. MySQL – Database service.
2. Flask – Backend application service.

### Step 1: Open Docker Compose File

Command:

```bash
vim docker-compose.yml
```

### Step 2: Add Docker Compose Configuration

```yaml
services:
  mysql:
    container_name: mysql
    image: mysql
    environment:
      MYSQL_DATABASE: "devops"
      MYSQL_ROOT_PASSWORD: "root"
    ports:
      - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - two-tier
    restart: always
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-uroot", "-proot"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 60s

  flask:
    build:
      context: .
    container_name: two-tier-backend
    ports:
      - "5000:5000"
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: root
      MYSQL_DB: devops
    networks:
      - two-tier
    depends_on:
      mysql:
        condition: service_healthy
    restart: always
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://localhost:5000/health || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 60s

volumes:
  mysql-data:

networks:
  two-tier:
    driver: bridge
```

### Step 3: Save the File

In Vim:

- Press `Esc`.
- Type `:wq`.
- Press `Enter`.

### Step 4: Verify Docker Compose File

Command:

```bash
cat docker-compose.yml
```

This displays the complete Docker Compose configuration.

---

# PART 3: UNDERSTANDING DOCKER COMPOSE COMMANDS

## 6. Start Docker Compose

Command:

```bash
docker compose up
```

### Explanation

- `docker compose` – Executes Docker Compose.
- `up` – Creates and starts the defined services.

This command starts the MySQL and Flask containers.

### Expected Output

```text
[+] Running 2/2
 ✔ Container mysql             Started
 ✔ Container two-tier-backend  Started
```

Note: The exact output depends on Docker Compose and the current project state.

**Important:** This command runs in the foreground and displays container logs in the terminal.

## 7. Run Docker Compose in Background

Command:

```bash
docker compose up -d
```

### Explanation

- `docker compose up` – Creates and starts services.
- `-d` – Detached mode.

Detached mode runs containers in the background, allowing us to execute other commands in the terminal.

### Expected Output

```text
[+] Running 2/2
 ✔ Container mysql             Started
 ✔ Container two-tier-backend  Started
```

### Difference Between Both Commands

| Command | Purpose |
|---|---|
| docker compose up | Runs containers in foreground. |
| docker compose up -d | Runs containers in background. |

---

# PART 4: VERIFY DOCKER COMPOSE

## 8. Check Running Containers

Command:

```bash
docker ps
```

This displays currently running Docker containers.

Expected output:

```text
CONTAINER ID   IMAGE          STATUS
xxxxxxxx       mysql          Up
xxxxxxxx       flask-image    Up
```

## 9. Check Compose Services

Command:

```bash
docker compose ps
```

Displays containers belonging to the current Compose project.

## 10. Check Docker Images

Command:

```bash
docker images
```

Displays all downloaded and locally built Docker images.

## 11. Check Docker Volumes

Command:

```bash
docker volume ls
```

Displays all Docker volumes.

You should see the MySQL data volume.

## 12. Check Docker Networks

Command:

```bash
docker network ls
```

Displays all available Docker networks.

You should see the Compose network associated with the project.

## 13. View Container Logs

Command:

```bash
docker compose logs
```

Displays logs from all Compose services.

To follow logs continuously:

```bash
docker compose logs -f
```

---

# PART 5: DOCKER COMPOSE IMPORTANT COMMANDS

## 14. Restart Services

Command:

```bash
docker compose restart
```

Restarts all services defined in the Compose file.

## 15. Stop and Remove Containers

Command:

```bash
docker compose down
```

Stops and removes the Compose containers and project network.

The named MySQL volume remains available.

## 16. Stop and Remove Containers with Volumes

Command:

```bash
docker compose down -v
```

Stops and removes containers, networks, and Compose-managed named volumes.

**Warning:** This removes the MySQL data volume and can permanently delete your database data.

## 17. Rebuild and Start Containers

Command:

```bash
docker compose up -d --build
```

Rebuilds the Flask image and starts the services in the background.

Use this command when you make changes to your Dockerfile or application image.

---

# PART 6: DOCKER COMPOSE ARCHITECTURE

## 18. Project Architecture

```text
              USER
                |
                |
           WEB BROWSER
                |
                |
            AWS EC2
                |
                |
         DOCKER COMPOSE
                |
          DOCKER NETWORK
             two-tier
                |
          -------------
          |           |
          |           |
        FLASK        MYSQL
       CONTAINER    CONTAINER
          |           |
          |           |
      BACKEND      DATABASE
      PORT 5000    PORT 3306
          |
          |
       VOLUME
      mysql-data
```

### How does it work?

1. Docker Compose reads the `docker-compose.yml` file.
2. It creates the required Docker network and volume.
3. It creates and starts the MySQL container.
4. It checks MySQL health.
5. After MySQL becomes healthy, it starts the Flask service.
6. Flask connects to MySQL using the hostname `mysql`.
7. The application becomes accessible through EC2 port 5000.

---

# PART 7: COMPLETE COMMAND REFERENCE

## 19. All Practical Commands

### Navigate to Project

```bash
cd ~/projects/two-tier-flask-app
```

### Install Docker Compose

```bash
sudo apt-get update

sudo apt-get install docker-compose-v2

docker compose version
```

### Create and Edit File

```bash
vim docker-compose.yml

cat docker-compose.yml
```

### Run Docker Compose

```bash
docker compose up

docker compose up -d
```

### Verify Containers

```bash
docker ps

docker compose ps

docker images

docker volume ls

docker network ls
```

### View Logs

```bash
docker compose logs

docker compose logs -f
```

### Restart Services

```bash
docker compose restart
```

### Stop Services

```bash
docker compose down
```

### Rebuild and Start

```bash
docker compose up -d --build
```

---

# 20. PRACTICAL LEARNING OUTCOME

By completing this practical, I learned:

1. What Docker Compose is.
2. Why Docker Compose is used.
3. How to install Docker Compose on Ubuntu EC2.
4. How to create a docker-compose.yml file.
5. How to configure MySQL and Flask services.
6. How to configure environment variables.
7. How to configure Docker networks.
8. How to configure persistent volumes.
9. How to use depends_on.
10. How to configure restart policies.
11. How to configure health checks.
12. How to start multiple containers using one command.
13. Difference between foreground and detached mode.
14. How to check containers, images, networks, and volumes.
15. How to stop and restart Docker Compose services.

**End of Practical – Docker Compose with Two-Tier Flask Application**


# Docker Registry Practical on AWS EC2

## Upload and Download Docker Images Using Docker Hub

# 1. Introduction to Docker Registry

### What is Docker Registry?

Docker Registry is a storage and distribution system used to store, manage, upload, and download Docker images.

It allows developers to share Docker images between different systems and servers.

### Why do we use Docker Registry?

- To store Docker images.
- To upload images from local systems to a remote registry.
- To download images from a registry.
- To share images with other developers.
- To maintain different versions of Docker images.
- To deploy applications on different servers.
- To avoid rebuilding images on every server.

### Popular Docker Registries

| Registry | Description |
| -------- | ----------- |
| Docker Hub | Cloud-based registry provided by Docker. |
| Amazon ECR | AWS service used to store Docker images. |
| GitHub Container Registry | Stores Docker images using GitHub. |
| Private Registry | Registry hosted privately by an organization. |

### Docker Registry Architecture

    Developer
        |
        |
    Dockerfile
        |
        |
    Docker Build
        |
        |
    Docker Image
        |
        |
    Docker Push
        |
        |
    Docker Hub (Registry)
        |
        |
    Docker Pull
        |
        |
    Another Server
        |
        |
    Docker Container

---

# PART 1: DOCKER IMAGE MANAGEMENT

## 2. Check Available Docker Images

Command:

```bash
docker images
```

This command displays all Docker images available on the local system.

Expected output:

```text
REPOSITORY          TAG       IMAGE ID
mysql               latest    xxxxxxxx
two-tier-backend    latest    xxxxxxxx
```

### Explanation

- `docker images` – Displays locally available Docker images.
- `REPOSITORY` – Image repository name.
- `TAG` – Image version.
- `IMAGE ID` – Unique image identifier.

## 3. Remove a Docker Image

Command:

```bash
docker rmi <image_id>
```

Example:

```bash
docker rmi abc12345
```

This command removes the specified Docker image.

### Explanation

- `docker rmi` – Removes a Docker image.
- `image_id` – ID of the image to remove.

**Important:** An image cannot normally be removed while it is being used by an existing container. Remove the dependent containers first.

## 4. Display All Docker Image IDs

Command:

```bash
docker images -aq
```

### Explanation

| Part | Meaning |
| ---- | ------- |
| docker images | Displays Docker images. |
| -a | Displays all images, including intermediate images. |
| -q | Displays only image IDs. |

This command is useful when we have multiple Docker images.

## 5. Remove Multiple Docker Images

Command:

```bash
docker rmi -f $(docker images -aq)
```

### Explanation

- `docker rmi` – Removes Docker images.
- `-f` – Forces image removal where permitted.
- `docker images -aq` – Retrieves image IDs.
- `$()` – Executes the command inside it and passes its output to the main command.

**Warning:** This command can remove many local images. Make sure you do not need them before executing it.

---

# PART 2: REMOVE DOCKER CONTAINERS

## 6. Check All Containers

Command:

```bash
docker ps -a
```

This command displays all containers, including running and stopped containers.

Expected output:

```text
CONTAINER ID   IMAGE          STATUS
xxxxxxxx       mysql          Up
xxxxxxxx       flask-image    Exited
```

## 7. Stop and Remove Docker Compose Containers

Command:

```bash
docker compose down
```

This command stops and removes containers and the Compose-created network.

The named volumes are retained by default.

**Note:** Removing containers does not automatically remove their Docker images.

After removing the containers, you can remove unused images if required.

---

# PART 3: BUILD DOCKER IMAGE

## 8. Navigate to Project Directory

Command:

```bash
cd ~/projects/two-tier-flask-app
```

This navigates to our Two-Tier Flask Application project directory.

## 9. Build Docker Image

Command:

```bash
docker compose up -d --build
```

### Explanation

| Part | Meaning |
| ---- | ------- |
| docker compose | Executes Docker Compose. |
| up | Creates and starts services. |
| -d | Runs containers in background. |
| --build | Builds images before starting services. |

### Why do we use --build?

We use `--build` to rebuild the application image when changes are made to the Dockerfile or application source code.

Expected output:

```text
[+] Building
[+] Running
Container mysql             Started
Container two-tier-backend  Started
```

---

# PART 4: LOGIN TO DOCKER HUB

## 10. What is Docker Hub?

Docker Hub is a cloud-based Docker Registry used to store, manage, upload, and download Docker images.

Website:

https://hub.docker.com/

### Step 1: Login to Docker Hub

Command:

```bash
docker login
```

Or:

```bash
docker login -u devdhareakshay2104
```

### Explanation

- `docker login` – Authenticates with Docker Hub.
- `-u` – Specifies the Docker Hub username.
- `devdhareakshay2104` – Docker Hub username.

Enter your password or Personal Access Token when prompted.

Expected output:

```text
Login Succeeded
```

### What is a Personal Access Token (PAT)?

A Personal Access Token is an authentication token used instead of your Docker Hub account password when logging in through the command line.

It can be generated from Docker Hub account settings.

---

# PART 5: TAG DOCKER IMAGE

## 11. What is Docker Image Tagging?

Docker image tagging is the process of assigning a repository name and version tag to an existing Docker image.

It helps identify images before uploading them to Docker Hub.

### Step 1: Check Local Docker Images

Command:

```bash
docker images
```

Example output:

```text
REPOSITORY          TAG       IMAGE ID
two-tier-backend    latest    xxxxxxxx
```

### Step 2: Tag the Docker Image

Command:

```bash
docker image tag two-tier-backend:latest devdhareakshay2104/two-tier-backend:latest
```

### Explanation

| Part | Meaning |
| ---- | ------- |
| docker image tag | Assigns a new tag to an image. |
| two-tier-backend | Existing local image name. |
| latest | Existing image tag. |
| devdhareakshay2104 | Docker Hub username. |
| two-tier-backend | Docker Hub repository name. |
| latest | New image version. |

### Step 3: Verify the Tagged Image

Command:

```bash
docker images
```

Expected output:

```text
REPOSITORY                              TAG       IMAGE ID
two-tier-backend                       latest    xxxxxxxx
devdhareakshay2104/two-tier-backend     latest    xxxxxxxx
```

Both names refer to the same local image ID.

---

# PART 6: PUSH IMAGE TO DOCKER HUB

## 12. What is Docker Push?

Docker Push is a command used to upload a local Docker image to a remote Docker Registry.

### Step 1: Push Image

Command:

```bash
docker push devdhareakshay2104/two-tier-backend:latest
```

### Explanation

- `docker push` – Uploads the Docker image.
- `devdhareakshay2104` – Docker Hub username.
- `two-tier-backend` – Repository name.
- `latest` – Image tag.

Expected output:

```text
The push refers to repository
docker.io/devdhareakshay2104/two-tier-backend

latest: pushed
```

The exact output may differ depending on whether image layers already exist in Docker Hub.

### Step 2: Verify Image on Docker Hub

1. Open https://hub.docker.com/
2. Login to your account.
3. Go to Repositories.
4. Open `two-tier-backend`.
5. Check the uploaded image and its tag.

Now your Docker image is stored in Docker Hub.

---

# PART 7: UPDATE DOCKER COMPOSE FILE

## 13. Update docker-compose.yml

After uploading the Flask backend image to Docker Hub, we can configure Docker Compose to use that remote image.

### Step 1: Open Docker Compose File

Command:

```bash
vim docker-compose.yml
```

### Step 2: Update Flask Service

Use the Docker Hub image name:

```yaml
flask:
  image: devdhareakshay2104/two-tier-backend:latest
  container_name: two-tier-backend
  ports:
    - "5000:5000"
  environment:
    MYSQL_HOST: mysql
    MYSQL_USER: root
    MYSQL_PASSWORD: root
    MYSQL_DB: devops
  networks:
    - two-tier
  depends_on:
    mysql:
      condition: service_healthy
  restart: always
```

### Explanation

- `image` – Specifies the Docker image from Docker Hub.
- `container_name` – Assigns a name to the container.
- `ports` – Maps EC2 port 5000 to container port 5000.
- `environment` – Defines database connection variables.
- `networks` – Connects Flask to the two-tier network.
- `depends_on` – Defines the MySQL service dependency.
- `restart` – Restarts the container automatically when required.

### Step 3: Save the File

In Vim:

1. Press `Esc`.
2. Type `:wq`.
3. Press `Enter`.

### Step 4: Verify Configuration

Command:

```bash
cat docker-compose.yml
```

---

# PART 8: PULL IMAGE FROM DOCKER HUB

## 14. What is Docker Pull?

Docker Pull is a command used to download a Docker image from a remote registry to the local system.

### Step 1: Pull Image from Docker Hub

Command:

```bash
docker pull devdhareakshay2104/two-tier-backend:latest
```

### Explanation

- `docker pull` – Downloads an image.
- `devdhareakshay2104` – Docker Hub username.
- `two-tier-backend` – Repository name.
- `latest` – Image tag.

Expected output:

```text
latest: Pulling from devdhareakshay2104/two-tier-backend

Status: Downloaded newer image
```

### Step 2: Verify Downloaded Image

Command:

```bash
docker images
```

You should now see:

```text
REPOSITORY                              TAG
devdhareakshay2104/two-tier-backend     latest
```

---

# PART 9: RUN APPLICATION USING DOCKER COMPOSE

## 15. Stop Existing Containers

Command:

```bash
docker compose down
```

This removes the existing Compose containers and network while retaining named volumes by default.

## 16. Start Application Again

Command:

```bash
docker compose up -d
```

Docker Compose uses the image specified in `docker-compose.yml`.

If the image is not available locally, Compose attempts to pull it from Docker Hub.

## 17. Verify Running Containers

Command:

```bash
docker ps
```

Expected output:

```text
CONTAINER ID   IMAGE                                      STATUS
xxxxxxxx       mysql                                      Up
xxxxxxxx       devdhareakshay2104/two-tier-backend:latest Up
```

## 18. Check Application Logs

Command:

```bash
docker compose logs
```

To follow logs continuously:

```bash
docker compose logs -f
```

---

# PART 10: DOCKER REGISTRY WORKFLOW

## 19. Complete Docker Registry Architecture

```text
        TWO-TIER FLASK PROJECT
                  |
                  |
             DOCKERFILE
                  |
                  |
              DOCKER BUILD
                  |
                  |
             DOCKER IMAGE
                  |
                  |
              DOCKER LOGIN
                  |
                  |
              DOCKER TAG
                  |
                  |
              DOCKER PUSH
                  |
                  |
           DOCKER HUB REGISTRY
                  |
                  |
              DOCKER PULL
                  |
                  |
          ANOTHER EC2 SERVER
                  |
                  |
            DOCKER COMPOSE
                  |
                  |
           FLASK CONTAINER
                  |
                  |
            MYSQL CONTAINER
```

### How does it work?

1. We create a Flask application.
2. We build a Docker image.
3. We login to Docker Hub.
4. We tag the local image with our Docker Hub username.
5. We push the image to Docker Hub.
6. We update the Docker Compose file with the remote image name.
7. We pull the image from Docker Hub.
8. We start the application using Docker Compose.
9. The Flask container communicates with MySQL through the Docker network.

---

# PART 11: COMPLETE COMMAND REFERENCE

## 20. All Practical Commands

### Navigate to Project

```bash
cd ~/projects/two-tier-flask-app
```

### Docker Image Management

```bash
docker images

docker images -aq

docker rmi <image_id>

docker rmi -f $(docker images -aq)
```

### Container Management

```bash
docker ps -a

docker compose down
```

### Build Application

```bash
docker compose up -d --build
```

### Docker Hub Login

```bash
docker login

docker login -u devdhareakshay2104
```

### Tag Image

```bash
docker image tag two-tier-backend:latest devdhareakshay2104/two-tier-backend:latest
```

### Push Image

```bash
docker push devdhareakshay2104/two-tier-backend:latest
```

### Pull Image

```bash
docker pull devdhareakshay2104/two-tier-backend:latest
```

### Update Docker Compose

```bash
vim docker-compose.yml

cat docker-compose.yml
```

### Start Application

```bash
docker compose up -d
```

### Verify Application

```bash
docker ps

docker images

docker compose ps

docker compose logs
```

---

# PART 12: IMPORTANT DIFFERENCES

## 21. Docker Push vs Docker Pull

| Docker Push | Docker Pull |
| ----------- | ----------- |
| Uploads an image. | Downloads an image. |
| Local system to registry. | Registry to local system. |
| Used to share images. | Used to obtain images. |

## 22. Docker Image vs Docker Registry

| Docker Image | Docker Registry |
| ------------ | --------------- |
| Contains application and its dependencies. | Stores and distributes Docker images. |
| Used to create containers. | Used to share images between systems. |
| Created using Docker build. | Receives images using Docker push. |

---

# 23. PRACTICAL LEARNING OUTCOME

By completing this practical, I learned:

1. What Docker Registry is.
2. What Docker Hub is.
3. Why Docker Registry is used.
4. How to check Docker images.
5. How to remove Docker images.
6. How to remove multiple Docker images.
7. How to manage Docker containers.
8. How to login to Docker Hub.
9. What a Personal Access Token is.
10. How to tag Docker images.
11. How to push images to Docker Hub.
12. How to verify uploaded images.
13. How to update docker-compose.yml with a remote image.
14. How to pull images from Docker Hub.
15. How to run an application using a Docker Hub image.
16. How to deploy containerized applications using Docker Compose.

**End of Practical – Docker Registry and Docker Hub on AWS EC2**

