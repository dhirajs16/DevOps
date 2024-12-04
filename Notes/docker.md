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
