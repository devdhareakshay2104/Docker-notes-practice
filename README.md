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
