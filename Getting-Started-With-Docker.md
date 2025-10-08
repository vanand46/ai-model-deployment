# Getting Started with Docker

## Introduction
This guide walks you through the basics of using Docker to run containers, build your own images, and understand how Dockerfiles work.

## Running Your First Container

If you’re new to Docker, start with the official Docker “Getting Started” example.

### Step 1: Download the Example Files

You can clone the example project from the Docker GitHub repository:

```bash
git clone https://github.com/docker/getting-started.git
cd getting-started
```

### Step 2: Run the Container
```bash
docker run -d -p 80:80 docker/getting-started
```
This command performs the following actions:
- Downloads the docker/getting-started image from Docker Hub (if it’s not already on your system)
- Runs it as a detached container (-d)
- Maps port 80 of the container to port 80 on your local machine

Now, open your browser and go to http://localhost:80

### Step 3: Build and Run Your Own Docker Image
If you make changes to the Dockerfile or the application code, rebuild the image before running it again:
```bash
docker build -t myapp:latest .
docker run -d -p 8080:80 myapp:latest
```
This command builds a new Docker image named myapp using the Dockerfile in the current directory and runs it on port 8080.

## Understanding the Dockerfile

Inside the example repository, you will find a file named `Dockerfile`.
A `Dockerfile` defines how to assemble a container image — it specifies the environment, dependencies, and configurations needed for your app.

### Example Dockerfile

```Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.11-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container
COPY . .

# Install any needed dependencies
RUN pip install -r requirements.txt

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
```
