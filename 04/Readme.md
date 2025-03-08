# Docker Guide: Introduction and Deploying a Node.js App

## What is Docker?

Docker is a platform that allows developers to package applications and their dependencies into lightweight, portable containers. These containers can run on any system that has Docker installed, ensuring consistency across different environments.

### Key Benefits of Docker:

- **Portability:** Works across different operating systems without compatibility issues.

- **Efficiency:** Uses less system resources than virtual machines.

- **Scalability:** Easily replicates and scales applications.

- **Consistency:** Ensures that the app runs the same way in development, testing, and production.



## Historical overview

- chroot (On Unix) Folder acts as an OS.

- FeeBSD Jail (chroot with hostname, ip address, users).

- ezjail (Just like a container manager for jails).

- Solaries containers (Intermediary between VMS and containers).

- Cgroups (Control groups by Google): Limit resources for a process.

- Linux container (LXC) : Used the previous tech

- Docker: Started as a manager on top of LXC then made thier own engine.

---

## Installing Docker

Before using Docker, you need to install it on your system.

1. **Download Docker:**

   - Visit [Docker's official website](https://www.docker.com/get-started)
   - Download and install Docker for your operating system.

2. **Verify Installation:**

   ```sh
   docker --version
   ```

   If Docker is installed correctly, this command will return the installed version.

---

## Understanding Basic Docker Commands

Before we start implementing a Dockerized application, here are some essential Docker commands:

### Docker Images and Containers:

- **List installed Docker images:**
  ```sh
  docker images
  ```
- **Remove an image:**
  ```sh
  docker rmi <image_id>
  ```
- **List running containers:**
  ```sh
  docker ps
  ```
- **List all containers (including stopped ones):**
  ```sh
  docker ps -a
  ```
- **Remove a container:**
  ```sh
  docker rm <container_id>
  ```

### Running and Managing Containers:

- **Run a container interactively:**
  ```sh
  docker run -it ubuntu bash
  ```
- **Run a container in detached mode:**
  ```sh
  docker run -d nginx
  ```
- **Stop a running container:**
  ```sh
  docker stop <container_id>
  ```
- **Restart a container:**
  ```sh
  docker restart <container_id>
  ```
- **View container logs:**
  ```sh
  docker logs <container_id>
  ```
- **Enter a running container:**
  ```sh
  docker exec -it <container_id> bash
  ```

### Docker Networking:

- **List networks:**
  ```sh
  docker network ls
  ```
- **Create a network:**
  ```sh
  docker network create my_network
  ```
- **Run a container on a specific network:**
  ```sh
  docker run --network=my_network nginx
  ```

These commands will help you manage Docker images, containers, and networks efficiently.

---

## Deploying a Node.js Application with Docker

### Step 1: Create a Simple Node.js Application

1. Create a new project folder:
   ```sh
   mkdir docker-node-app && cd docker-node-app
   ```
2. Initialize a Node.js project:
   ```sh
   npm init -y
   ```
3. Install Express.js:
   ```sh
   npm install express
   ```
4. Create an `index.js` file and add the following code:
   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/', (req, res) => {
       res.send('Hello from Dockerized Node.js App!');
   });

   app.listen(PORT, () => {
       console.log(`Server running on port ${PORT}`);
   });
   ```
5. Test the application locally:
   ```sh
   node index.js
   ```
   Open your browser and go to `http://localhost:3000` to see the output.

---

### Step 2: Create a Dockerfile

Create a `Dockerfile` in the root folder of your project and add the following content:

```dockerfile
# Use an official Node.js runtime as the base image
FROM node:18

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose the application port
EXPOSE 3000

# Command to run the application
CMD ["node", "index.js"]
```

---

### Step 3: Create a `.dockerignore` File

To exclude unnecessary files, create a `.dockerignore` file with the following content:

```
node_modules
npm-debug.log
Dockerfile
.dockerignore
```

---

### Step 4: Build the Docker Image

Run the following command to build the Docker image:

```sh
docker build -t node-docker-app .
```

This command creates a Docker image named `node-docker-app`.

---

### Step 5: Run the Docker Container

After building the image, start a container using:

```sh
docker run -p 4000:3000 node-docker-app
```

- `-p 4000:3000` maps port 3000 inside the container to port 4000 on your local machine.
- Now, open `http://localhost:4000` in your browser to see the running app.

---

### Step 6: Stop and Remove Containers

To stop the container, press `Ctrl + C` or run:

```sh
docker ps  # Check running containers
docker stop <container_id>
```

To remove a container after stopping it:

```sh
docker rm <container_id>
```

---

### Step 7: Push the Image to Docker Hub (Optional)

To share your Docker image, follow these steps:

1. Log in to Docker Hub:
   ```sh
   docker login
   ```
2. Tag the image:
   ```sh
   docker tag node-docker-app your-dockerhub-username/node-docker-app
   ```
3. Push the image:
   ```sh
   docker push your-dockerhub-username/node-docker-app
   ```

Your image is now available on Docker Hub!

---

## Conclusion

You have successfully learned how to:

- Set up Docker.
- Understand basic Docker commands.
- Create and Dockerize a Node.js application.
- Build and run a Docker container.
- Push a Docker image to Docker Hub.

Now, you can use Docker to deploy applications efficiently across different environments. Happy coding!

