---
layout: post
title: "Deploying a Three-Tier Application with Docker"
author: Aditya Shah
date: 2024-04-24 08:00:00 +0800
categories: [Docker, Deployment, Tutorial]
tags: [Docker, Deployment, MERN Stack]
excerpt: "Learn how to deploy a full-stack web application using Docker containers."


---

Hey everyone,

In this blog post, we'll dive into deploying a full-stack web application using Docker. Our application will be divided into three microservices:

1. Frontend Container of ReactJs
2. Backend Container of NodeJs
3. Database Container of MongoDB

## Understanding Docker

Before we begin with the deployment, let’s grasp the essence of Docker and why it's indispensable. Docker is a platform that empowers developers to develop, test, and deploy applications in containers. These containers are lightweight, standalone, and executable packages of software that encapsulate everything needed to run an application. They offer consistency across different environments, making deployment a breeze.

Docker provides tools like Docker Compose to define and run multi-container applications, which we'll utilize in this blog.

### 1. Setting Up the Application

#### 1.1. Clone the Application

Start by cloning the application from GitHub. I've used an application from GitHub, which you can find [here](https://github.com/arpitmathur2412/GFG-HACK). Ensure it matches the same tech stack.

#### 1.2. File Structure

Here's the file structure of the application:

GFG-HACK/
├── client/
└── server/


### 2. Creating Docker Hub Repository

Head over to [Docker Hub](https://hub.docker.com/) and create a new repository to store your Docker images.
Make it public or private as per your preference. Remember the repository name for later use.

### 3. Dockerizing Each Service

#### 3.1. Dockerfile for ReactJs Server

Navigate to the client directory:

cd client

Create a Dockerfile:

touch Dockerfile

Open the Dockerfile and add the following content:

FROM node:16

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npx", "serve", "-s", "build"]

Build and push the Docker image:

docker build -t AdityaShah04/gfg-hack:client .
docker push AdityaShah04/gfg-hack:client


3.2. Dockerfile for NodeJs Server

Navigate to the server directory:

cd ../server

Create a Dockerfile:

touch Dockerfile

Open the Dockerfile and add the following content:

FROM node:16

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 5000

CMD ["npm", "start"]

Build and push the Docker image:

docker build -t AdityaShah04/gfg-hack:server .

docker push AdityaShah04/gfg-hack:server

4. Running the Application

Now that we have all our images ready, let's run our application:

docker run -dp 3000:3000 AdityaShah04/gfg-hack:client

docker run -dp 27017:27017 mongo

Create a Docker network and attach it to the MongoDB container. Then, attach the same network to the server container for communication between server and database.


docker network create gfg-hack-network

docker network connect gfg-hack-network <mongo-container-id>

docker network connect gfg-hack-network <server-container-id>

Now, run the server container:

docker run -dp 5000:5000 AdityaShah04/gfg-hack:server

Once all containers are up, you can access the app at http://localhost:3000.

Conclusion

By containerizing our MERN stack application with Docker, we've achieved a portable, efficient, and scalable development environment. 
This approach simplifies deployment workflows and lays the groundwork for seamless production deployment.

Remember, this is just the beginning! Explore further optimizations for production environments, such as volume management and container orchestration tools like Kubernetes.

Happy coding!
