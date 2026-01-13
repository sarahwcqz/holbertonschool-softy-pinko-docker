# Docker

## Context
Docker is a platform that allows you to containerize your applications, meaning that you can package them into portable, self-contained environments which can run anywhere. This means that you can quickly move your application from one environment to another, such as from your local computer to a server, without worrying about dependencies or configuration issues. Docker achieves this by using containers, which are isolated environments that contain everything an application needs to run, such as libraries, dependencies, and configurations. Docker containers are lightweight and can be started and stopped quickly, making them ideal for modern software development and deployment. With Docker, you can also quickly scale your application by running multiple containers of the same application on different hosts (or the same host, as we will do in this project), and manage them using Docker Compose or other orchestration tools.

Ultimately, what you will create in this project is an infrastructure for an application that utilizes a reverse proxy, a load balancer, two application servers, and one front-end server.

Let’s consider the following design:



### High-level Design
In this design, there is a single server that acts as the entry point for your application. That server acts as a reverse proxy server, which routes traffic to either the API servers or the front-end static-content server; it also acts as a load balancer to balance traffic between the two API servers. When traffic comes in, the server will determine which service it needs to go to (either the front-end static-content server or the API server) and:

- If the request is to be routed to the front-end static-content server, it will do so and any static content that is returned from the front-end static-content server to the proxy server will then be sent to the client. The client isn’t directly communicating with the front-end static-content server.

- If the request is to be routed to the API server, then it will first go through a load-balancing algorithm to determine which API server to send the request to. We will be using the basic Round Robin load-balancing algorithm for our site. Once the request is routed to an API server and the response is sent back to the proxy server, it will then be sent to the client. The client isn’t directly communicating with either of the API servers.

What is Round Robin load balancing? Round Robin load balancing is a method of distributing traffic across multiple servers or nodes in a network. In this approach, the load balancer assigns requests to each server in a rotating manner, meaning that each server takes turns serving the requests. For example, if there are three servers A, B, and C, the first request will be sent to server A, the second to server B, the third to server C, the fourth to A, the fifth to B, and so on, while the cycle repeats. This ensures that each server receives an equal share of the traffic, and can prevent any one server from becoming overwhelmed with requests. Round Robin load balancing is a simple and effective way to distribute traffic, but it may not be suitable for all applications, particularly those with varying workload patterns or resource requirements. For our learning purposes in this project, it is a sufficient algorithm to implement for our load-balancing needs.

## Tasks
**0. Create Your First Docker Image**
<br>To create a Docker image, you will need to utilize a Dockerfile. Create a Dockerfile that:
- Is based on the latest ubuntu
- Update APT using apt-get update
- Upgrade currently installed software through APT using apt-get upgrade -y
- Once built, you can run the Docker image in a container and it will echo “Hello, World!” on the terminal.

**1. Back-end**<br>
The goal of this task is to create a new Docker image that runs a Flask API.

**2. Front-end** <br>Set up a front-end / back-end architecture by moving the Flask API into task2/back-end and serving a static front-end with Nginx in task2/front-end, using a Dockerized Nginx server exposed on port 9000.

**3. Connecting the Front-end and Back-end**<br>
Connect the front-end to the back-end by fetching dynamic data from the Flask API using JavaScript (AJAX), and update the back-end to allow cross-origin requests with flask-cors, enabling communication between two Docker containers running separately.

**4. Making it Simpler with Docker Compose**<br>
Use Docker Compose to define and run the front-end and back-end containers together by creating a docker-compose.yml file in task4, allowing the entire application to be built and started with docker-compose build and docker-compose up.

**5. Proxy Server**<br>
Add an Nginx reverse proxy in front of the front-end and back-end services, update Docker Compose to route all traffic through this proxy on port 80, and modify the front-end to call the API via /api instead of accessing the back-end directly.

**6. Scale Horizontally**<br>
Scale the back-end by running multiple API containers using Docker Compose and configure Nginx to load-balance requests (round-robin) between them, documenting the exact docker-compose command used to start two API servers.
