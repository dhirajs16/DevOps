<img src="https://th.bing.com/th/id/R.f5f31dde1e196d8a9be784f3ba95ec42?rik=QNTo%2fA3NdcoeHA&pid=ImgRaw&r=0" />

# Docker CLI Commands

## 1. Pulling Images

Pull an image from Docker Hub:

```bash
docker pull [image_name]:[tag]
```

## 2. Running Containers

Run a container from an image:

```bash
docker run [options] [image_name]:[tag]

# Common options:
# -d: Run container in detached mode
# -p host_port:container_port: Map a host port to a container port
# --name: Assign a name to the container
```

## 3. Viewing Images and Containers

List all images:

```bash
docker images
```

List running containers:

```bash
docker ps
```

List all containers (including stopped):

```bash
docker ps -a
```

## 4. Managing Containers

Stop a running container:

```bash
docker stop [container_id/name]
```

Start a stopped container:

```bash
docker start [container_id/name]
```

Remove a container:

```bash
docker rm [container_id/name]
```

## 5. Container Logs and Execution

View container logs:

```bash
docker logs [container_id/name]
```

Execute a command in a running container:

```bash
docker exec -it [container_id/name] [command]
```

## 6. Building Images

Build an image from a Dockerfile:

```bash
docker build -t [image_name]:[tag] [path_to_dockerfile]
```

## 7. Network and Volume Commands

List networks:

```bash
docker network ls
```

Create a volume:

```bash
docker volume create [volume_name]
```

## 8. System Commands

Display Docker system-wide information:

```bash
docker info
```

Remove unused data:

```bash
docker system prune
```

## 9. Removing Images and Containers

Stop all running containers:

```bash
docker stop $(docker ps -aq)
```

Remove all containers:

```bash
docker rm $(docker ps -aq)

Or,
docker container prune    #Toremove all the stopped containers
```

Remove a specific image:

```bash
docker rmi [image_id/name]
```

Remove all unused images:

```bash
docker image prune -a
```

Remove all unused containers, networks, images (both dangling and unreferenced), and optionally, volumes:

```bash
docker system prune -a
```

Note: Always use caution when using prune commands, as they can remove significant amounts of data.

## 10. Working with Private Registries

Login to a private registry:

```bash
docker login [registry_url]
```

Pull an image from a private registry:

```bash
docker pull [registry_url]/[image_name]:[tag]
```

Push an image to a private registry:

```bash
docker push [registry_url]/[image_name]:[tag]
```

Tag an image with a new name:

```bash
docker tag [source_image]:[tag] [new_image_name]:[new_tag]
```

## 11. Saving and Loading Images

Save an image to a tar archive:

```bash
docker save -o [output_file.tar] [image_name]:[tag]
```

Note: Saving and loading images can be useful for transferring images between systems without using a registry.

## 12. Creating and Building Docker Images

To build an image from your Dockerfile:

```bash
# Build your image from a dockerfile in the current directory
docker build .

# Build the image (in the same directory as the Dockerfile)
docker build -t my-custom-image:v1 .

# Build the image from a different directory
docker build -t my-custom-image:v1 /path/to/dockerfile/directory

# Assign a name and tag while building
docker build -t username/repository:tag .
```

To create your own Docker image, you need to create a Dockerfile:

```bash
# Create a Dockerfile
touch Dockerfile

# Edit the Dockerfile with your preferred text editor
nano Dockerfile
```

A basic Dockerfile structure:

```
# Use an official base image
FROM ubuntu:20.04

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages
RUN apt-get update && apt-get install -y \
    package1 \
    package2

# Add shell commands
RUN echo "Hello from the Dockerfile" > greeting.txt

# Command to run when the container starts
CMD ["bash"]
```

Note: The '.' at the end of the build command specifies the build context (current directory). Replace it with the path to your Dockerfile if it's in a different location.

After building, you can run your custom image:

```bash
docker run -it my-custom-image:v1
```

## 13. Understanding the RUN Command in Docker

The RUN command is used in Dockerfiles to execute commands during the image build process. It creates a new layer in the image.

Key points about the RUN command:

- It executes commands in a new layer on top of the current image and commits the results.
- Each RUN command creates a new layer in the Docker image.
- It's often used to install software packages, run scripts, or perform other setup tasks.

Syntax:

```
RUN <command>
# or
RUN ["executable", "param1", "param2"]
```

Examples:

```
# Update package lists and install software
RUN apt-get update && apt-get install -y python3

# Create directories
RUN mkdir -p /app/data

# Run a shell script
RUN ./setup.sh

# Using the exec form
RUN ["pip", "install", "flask"]
```

Best practices:

- Combine multiple RUN commands using && to reduce the number of layers and image size.
- Use the exec form (["executable", "param1", "param2"]) for better parsing and to avoid issues with shell string munging.
- Clean up unnecessary files after installations to keep the image size small.

<hr>

[Video 1](https://www.youtube.com/watch?v=7JZP345yVjw&list=PLdpzxOOAlwvLjb0vTD9BXLOwwLD_GWCmC)

### Q.1. Why docker containers are light weight?

It is so because docker container don’t have their own dedicated complete OS (like VMs) they are just the collection of source code, libraries and system dependencies. In fact it uses the base OS of the host over which the container is running.

<img src="https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png" />

<hr>

[Video 2](https://www.youtube.com/watch?v=wodLpta-hoQ&list=PLdpzxOOAlwvLjb0vTD9BXLOwwLD_GWCmC&index=2)

### **Container Basics**

- **Shared Kernel**: Docker containers share the host operating system's kernel, which allows them to run lightweight and efficiently. Unlike virtual machines (VMs) that require their own full OS, containers operate directly on the host's kernel, making them faster and less resource-intensive.
- **Limited OS Files**: Containers do not include a complete operating system; instead, they contain only the necessary binaries and libraries required for the applications to run. This means that while they may be based on OS images (like Ubuntu or Alpine), they do not replicate the entire OS structure. The kernel is shared among all containers running on the same host.
    - **Some of the necessary binaries and libraries files which they have on their own:**
    
    ```
    /bin: contains binary executable files, such as the ls, cp, and ps commands.
    
    /sbin: contains system binary executable files, such as the init and shutdown commands.
    
    /etc: contains configuration files for various system services.
    
    /lib: contains library files that are used by the binary executables.
    
    /usr: contains user-related files and utilities, such as applications, libraries, and documentation.
    
    /var: contains variable data, such as log files, spool files, and temporary files.
    
    /root: is the home directory of the root user.
    ```
    
    - **Files and Folders that containers use from host operating system:**
    
    ```
    The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.
    
    Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.
    
    System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.
    
    Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.
    
    Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
    
    ```
    
    - These set of files are collectively needed for the execution of containers. The reason they are used like this to maintain isolation.

### **Security and Isolation**

- **Isolation Mechanisms**: To maintain security, Docker employs several isolation techniques, including Linux namespaces and cgroups. These ensure that processes in one container cannot see or interfere with processes in another container or the host. Each container operates in its own isolated environment, which is critical for security.
- **Vulnerability Concerns**: Your concern about security is valid. If one container is compromised, it could potentially affect others since they share the same kernel. This is why Docker implements various security measures, such as enhanced container isolation features that map container users to unprivileged users on the host, limiting what a compromised container can access.
- **Best Practices**: To enhance security further, it is recommended to run containers with the least privileges necessary and to limit their capabilities. This practice minimizes the risk of one container affecting others or accessing sensitive data on the host[7][8].

## **Architecture of Docker**

<img src="https://user-images.githubusercontent.com/43399466/217507877-212d3a60-143a-4a1d-ab79-4bb615cb4622.png" />



### 1. **Docker Client**

This is the **command-line interface** where you type `docker` commands. It communicates with the Docker Engine using the Docker API.

### 2. **Docker Engine**

The heart of Docker, which consists of two main components:

- **Docker Daemon (dockerd)**: Runs on the host machine, managing Docker objects like images, containers, networks, and volumes. It is the one who run the commands coming from the Docker Client.
- **REST API**: Used by the Docker Client to interact with the Docker Daemon.

### 3. **Docker Images**

These are read-only templates used to create containers. Think of them as blueprints of your application that include everything needed to run it: code, libraries, and dependencies.

### 4. **Docker Containers**

Containers are lightweight, portable, and isolated environments where your applications run. They are created from Docker images and can be run, started, stopped, moved, and deleted.

### 5. **Docker Registries**

These are storage and distribution systems for Docker images. The most well-known public registry is Docker Hub, but you can also set up your own private registry.

### Summary

1. **Docker Client**: Your interface to interact with Docker.
2. **Docker Engine**: Runs and manages Docker components.
3. **Docker Images**: Blueprints for your application.
4. **Docker Containers**: The running instances of your images.
5. **Docker Registries**: Where you store and share your images.

In essence, Docker streamlines development and deployment by ensuring your application runs the same way everywhere—whether on your local machine or in the cloud.
## Workflow

### Dockerfile:

- **Role**: A script with instructions to set up the environment, copy files, install dependencies, and specify the command to run when the container starts.
- **Purpose**: Defines how to build a Docker image.

### Docker Client (CLI):

- **Role**: The command-line tool you use to interact with Docker.
- **Tasks**:
    - **Building Image**: Uses `docker build` to create an image from a Dockerfile.
    - **Pulling Image**: Uses `docker pull` to download images from a registry.
    - **Running Container**: Uses `docker run` to create and start a container from an image.

### Workflow:

1. **Write Dockerfile**: Create a Dockerfile with setup instructions.
2. **Build Image**: Use the Docker Client to build the image from the Dockerfile.
3. **Push/Pull Image**: Use the Docker Client to push the image to a registry or pull an image from the registry.
4. **Run Container**: Use the Docker Client to run the image, creating a container.

So, the Dockerfile lays out the blueprint, while the Docker Client handles the building, pulling, and running processes.
