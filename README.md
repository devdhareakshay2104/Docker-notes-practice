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
