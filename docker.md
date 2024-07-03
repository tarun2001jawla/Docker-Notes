# Docker Mastery: with Kubernetes +Swarm

Docker Innovations:

1. Docker Image-(Build)
2. Docker Registry-(Ship)
3. Docker Container-(Run)
These three go together like bread and butter Build ship and run.



## What Problem does Docker solve?
1. Problem of isolation 
2. Problem of environments 
3. The problem of speed (speed of business or speed of software delivery)
The main advantage of using containers is that it gives speed to software development.

## Common Docker Commands
- docker version - to check the version of the docker
- docker info - most config values of docker-engine
- docker - to list all commands in the  docker 
- docker CLI Structure : docker <command> <sub-command> (option)
## Image vs Container:
### Image:
An image is binaries, source code, and libraries that all make up our application.

An image is the application we want to run.



### Container:
A container is an instance of that image running as a process.

We can run many containers with the same image.



#### Running a container:
```
docker run -p 80:80 nginx
docker run -p <public port>:<container port> nginx
```
1. Downloaded image 'nginx' from docker hub.
2. Started a new container from that image.
3. Opened port 80 on the host IP.
4. Routes that traffic to container IP, port 80.


#### To run the container in the background:
```
docker run -p 80:80 --detach nginx
```
#### To list out the containers:
```
docker container ls (for running)
docker container ls -a (for all)
```
#### To stop a running container:
```
docker container stop [id] 
docker container stop f079009ddda7
```
#### To see logs of the container:
```
docker container logs [name/id]
docker container webhost or 804b82cbae02
```
#### To Display the running processes of a container:
```
docker container top [name/id]
docker container webhost or 804b82cbae02
```
#### To remove the containers:
```
docker container rm [id1] [id2].....[idn]
```
However, we can't remove a running container directly if we do so we will get this error:

```
Error response from daemon: 
cannot remove container "/webhost": container is running: 
stop the container before removing or force remove
```
to remove this we can either stop the container first by command : 

`docker container stop [id] `  or we can force remove by this 

command: 

```
docker container rm [id] --force 
```
﻿﻿ 

## What happens when we do 'docker container run'?
1. First, it looks for the image locally in the image cache.
2. Then looks in the remote repo (docker hub).
3. Downloads the latest version of that image
4. Creates  a new container based on that image and prepares to start
5. Gives it a virtual IP on a private network inside the docker engine 
6. opens up port 80 on the host and forwards traffic to port 80 in the container.
7. Starts the container by using the CMD in the image the dockerfile.
```
docker container run -p [PORT:PORT] --name [name] 
-d [imagename] [imagename] -T 
```
# Containers vs. virtual machines:
Virtualization is the process in which a system singular resource like RAM, CPU, Disk, or Networking can be ‘virtualized’ and represented as multiple resources.

Containers are lightweight software packages that contain all the dependencies required to execute the contained software application:

- Just processes running on our host OS
- Limited to what resources they can access
- Exit when the process stops 
## What's Going On In Containers: CLI Process Monitoring:
```
docker container top [id]
```
Display the running processes of a container

```
docker container inspect [id/name]
```
Shows the meta-data about the container (startup, volume, config, networking, etc)

```
docker container stats [id/name]
```
Display a live stream of container(s) resource usage statistics.



##  Getting a Shell Inside Containers: 
```
docker container run -it ngninx
```
Allows us to run a container interactively

```
docker container exec -it [container name] [command]
docker container exec -it sql bash 
```
## Docker Networking:
- By default, each network connects to a private virtual network "bridge".
- Each virtual network routes through a NAT firewall on the host IP.
- All containers on the virtual network can talk to each other without -p
We can:

1. Make new virtual networks
2. Skip virtual network and use host IP(--net=host)
3. Use different docker network drivers to gain abilities
and much more...



### To Get the IP Of a container:
```
docker container inspect --format 
"{{ .NetworkSettings.IPAddress }}" [container name]
```
###  Docker Networks: CLI Management of Virtual Networks:
#### To get the list of networks:
```
docker network ls
```
#### Inspect a Network:
```
docker network inspect 
```
#### Create a network:
```
docker network create --driver
```
#### Attach/Detach a network to a container:
```
docker network connect/disconnect 
```
### Docker Network: DNS
- Containers shouldn't rely on IP's for inter-communication
- DNS for the friendly name is built in if you use custom networks
- Always create custom networks


#### CLI Testing:
```
docker container run --rm
```
The `--rm` flag is there to tell the Docker Daemon to clean up the container and remove the file system after the container exits.



# Images in Docker:
### To Check the history of an image:
```
docker history node
```
### To get the metadata about an image:
```
docker inspect nginx
```
#### this will give output like this: 
```
{
    "Id": "sha256:e784f4560448b14a66f55c26e1b4dad2c2877cc73d001b7cd0b18e24a700a070",
    "RepoTags": ["nginx:latest"],
    "Created": "2024-05-03T19:49:21Z",
    "ExposedPorts": {
        "80/tcp": {}
    },
    "Env": [
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
        "NGINX_VERSION=1.25.5",
        "NJS_VERSION=0.8.4",
        "NJS_RELEASE=3~bookworm",
        "PKG_RELEASE=1~bookworm"
    ],
    "Cmd": ["nginx", "-g", "daemon off;"],
    "Entrypoint": ["/docker-entrypoint.sh"],
    "Labels": {
        "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
    },
    "RootFS": {
        "Layers": [
            "sha256:5d4427064ecc46e3c2add169e9b5eafc7ed2be7861081ec925938ab628ac0e25",
            "sha256:fc1cf9ca5139883943cc519cc3c57f0855395618f56d6431490fa735461156f1",
            "sha256:747b290aeba888738176dcbf7382eb0f660f27e785b839592918b8ed291d5792",
            "sha256:9f4d73e635f122031c04047f0e87fb224bef098a28700439bb4e72d0619aaad6",
            "sha256:56f8fe6aedcdee31bdee17249b1e18434ce7ab5a1814e2193773b54d7f9db39a",
            "sha256:7d2fd59c368c60f74214fc47399dcc35ef8513e26890a0c6004835bceabeb4c6",
            "sha256:14773070094ddc0debcea4f38f0daa7dd8116858387e0c238fa96ae8047bc07e"
        ]
    },
    "Architecture": "amd64",
    "Os": "linux"
}
```
A Container is just a single read/write layer on top of an image. 



### Docker Image Tag:
```
docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
docker image tag myapp:v1.0 myrepository/myapp:latest
```
Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

The default tag is "latest".



# Building Docker images: The Dockerfile basics:


- A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.
- Docker can build images automatically by reading the instructions from a Dockerfile.
- Docker runs instructions in a Dockerfile in order. A Dockerfile **must begin with an **`FROM` ** instruction**.
- The `FROM`  instruction specifies the [﻿parent image](https://docs.docker.com/glossary/#parent-image) from which you are building.
### Here is the format of the Dockerfile:
The instruction is not case-sensitive. However, the convention is for them to be UPPERCASE to distinguish them from arguments more easily.

```
# Comment
INSTRUCTION arguments
```
```
# Specifies the base image to use for subsequent instructions
FROM ubuntu:20.04

# Sets environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Adds metadata to the image, such as maintainer information
LABEL maintainer="you@example.com"

# Executes commands in a new layer and commits the results
RUN apt-get update && apt-get install -y \
    curl \
    vim \
    && rm -rf /var/lib/apt/lists/*

# Sets the working directory for the following instructions
WORKDIR /usr/src/app

# Copies files or directories from the host to the container
COPY . /usr/src/app

# Informs Docker that the container listens on the specified network ports at runtime
EXPOSE 80

# Sets the user name or UID to use when running the image and for any RUN, CMD, and ENTRYPOINT instructions that follow
USER nobody

# Creates a mount point with the specified path and marks it as holding externally mounted volumes from native host or other containers
VOLUME /usr/src/app/data

# Provides defaults for an executing container, e.g., executable or parameters
CMD ["nginx", "-g", "daemon off;"]

# Allows you to configure a container that will run as an executable
ENTRYPOINT ["nginx", "-g", "daemon off;"]

# Defines a variable that users can pass at build-time to the builder
ARG VERSION=1.0
```
In the example above, each instruction creates one layer:

1. **FROM**: Specifies the base image to use for subsequent instructions.
2. **ENV**: Sets environment variables.
3. **LABEL**: Adds metadata to the image, such as maintainer information.
4. **RUN**: Executes commands in a new layer and commits the results.
5. **WORKDIR**: Sets the working directory for the following instructions.
6. **COPY**: Copies files or directories from the host to the container.
7. **EXPOSE**: Informs Docker that the container listens on the specified network ports at runtime.
8. **USER**: Sets the user name or UID to use when running the image and for any `RUN` , `CMD` , and `ENTRYPOINT`  instructions that follow.
9. **VOLUME**: Creates a mount point with the specified path and marks it as holding externally mounted volumes from the native host or other containers.
10. **CMD**: Provides defaults for an executing container, e.g., executable or parameters.
11. **ENTRYPOINT**: Allows you to configure a container that will run as an executable.
12. **ARG**: Defines a variable that users can pass at build-time to the build
When you run an image and generate a container, you add a new writable layer, also called the container layer, on top of the underlying layers.

All changes made to the running container, such as writing new files, modifying existing files, and deleting files, are written to this writable container layer.



## Docker system commands:
- Show docker disk usage:
```
docker system df
```
```
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          8         8         1.793GB   0B (0%)
Containers      18        0         324.2MB   324.2MB (100%)
Local Volumes   12        4         1.052GB   829.7MB (78%)
Build Cache     144       0         5.933GB   5.933GB
```
- Remove unused data:
```
docker system prune 
```
This will remove:

- all stopped containers
- all networks not used by at least one container
- all dangling images
- unused build cache
```
docker image prune
```
This will remove all dangling images.

```
docker container prune
```
 This will remove all stopped containers.



---

# Persistent Data: Volumes
## Container lifetime and persistent data:
- Containers are usually immutable and ephemeral
## Volumes:
Volumes are the preferred mechanism for persisting data generated by and used by Docker containers.

- Volumes are easier to back up or migrate than bind mounts.
- You can manage volumes using Docker CLI commands or the Docker API.
- Volumes work on both Linux and Windows containers.
- Volumes can be more safely shared among multiple containers.
- Volume drivers let you store volumes on remote hosts or cloud providers, encrypt the contents of volumes, or add other functionality.
- New volumes can have their content pre-populated by a container.
- Volumes on Docker Desktop have much higher performance than bind mounts from Mac and Windows hosts.
In addition, volumes are often a better choice than persisting data in a container's writable layer, because a volume doesn't increase the size of the containers using it, and the volume's contents exist outside the lifecycle of a given container.



```
docker volume create [OPTIONS] [VOLUME]
```
Creates a new volume that containers can consume and store data in. If a name is not specified, Docker generates a random name.

Example:

```
$ docker volume create hello
>>hello
$ docker run -d -v hello:/world busybox ls /world
```
## Bind Mounting:
- When you use a bind mount, a file or directory on the host machine is mounted into a container.
- Two locations pointing to the same file.
- The file or directory is referenced by its absolute path on the host machine. 
```
docker run -d \
-it \
--name devtest \
--mount type=bind,source="$(pwd)"/target,target=/app \
nginx:latest
```


# Docker Compose:
- Docker Compose is a tool for defining and running multi-container applications.
- It is the key to unlocking a streamlined and efficient development and deployment experience.
## Docker Compose YAML:
- In order to do something useful with containers, they have to be arranged - orchestrated - as part of an application, usually referred to as an 'application'.
- There are multiple ways of orchestrating a Docker application, but **Docker Compose** is probably the most human-friendly. It's what we use for our local development environments.
- To configure the orchestration, Docker Compose uses a `docker-compose.yml`  file. It specifies what images are required, what ports they need to expose, whether they have access to the host filesystem, what commands should be run when they start up, and so on.
```
# Version of the Docker Compose file syntax
version: '3.8'

# Define services (containers)
services:
  
  # MySQL service
  mysql:
    # Use the official MySQL Docker image from Docker Hub
    image: mysql:latest
    # Define environment variables for MySQL container
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: workshopDB
      MYSQL_USER: db_user
      MYSQL_PASSWORD: db_password
    # Mount a volume to persist MySQL data
    volumes:
      - mysql_data:/var/lib/mysql
    # Expose port for MySQL
    ports:
      - "3306:3306"
      
  # Your application service (replace "your_app" with your actual service name)
  your_app:
    # Define the image for your application
    image: your_app_image:latest
    # Define environment variables for your application container
    environment:
      DB_HOST: mysql
      DB_PORT: 3306
      DB_USER: db_user
      DB_PASSWORD: db_password
      DB_NAME: workshopDB
    # Define volumes to mount if needed
    volumes:
      - ./your_app_directory:/app
    # Depends on MySQL service, ensures MySQL container is started first
    depends_on:
      - mysql

# Define volumes to persist data
volumes:
  mysql_data:
```
Explanation:

- **version:** Specifies the version of the Docker Compose file syntax being used.
- **services:** Defines the services (containers) that compose your application.
    - **MySQL:** Specifies the MySQL service.
        - **image:** Specifies the Docker image to use for the MySQL service.
        - **environment:** Sets environment variables required for the MySQL container. In this example, it sets the root password, creates a database, and creates a user with an associated password.
        - **volumes:** Mounts a volume to persist MySQL data. This ensures that data is not lost when the container is stopped.
        - **ports:** Exposes the MySQL port to the host machine.
    - **your_app:** Specifies your application service. Replace "your_app" with your actual service name.
        - **image:** Specifies the Docker image for your application.
        - **environment:** Sets environment variables required for your application container. Here, we set variables like the database host, port, user, password, and database name.
        - **volumes:** Mounts volumes if needed. This example mounts the application directory to the `/app`  directory in the container.
        - **depends_on:** Defines a dependency on the MySQL service, ensuring that the MySQL container is started before your application container.
- **volumes:** Defines volumes to persist data. In this example, it defines a volume named `mysql_data`  for MySQL data persistence.
## Docker Compose CLI:
```
docker-compose up
```
This command reads the `docker-compose.yml` file in the current directory and starts the defined services (containers). If the services are not already built, Docker Compose builds them first.



```
docker-compose down
```
This command stops and removes the containers defined in the `docker-compose.yml` file. It also removes any networks created by `docker-compose up`.



# Docker Swarm
A Docker Swarm is a container orchestration tool running the Docker application.  It has been configured to join together in a cluster. 

The activities of the cluster are controlled by a swarm manager, and machines that have joined the cluster are referred to as nodes.

### Key takeaways:
- A Docker Swarm is a group of either physical or virtual machines that are running the Docker application and that have been configured to join together in a cluster.
- The activities of the cluster are controlled by a swarm manager, and machines that have joined the cluster are referred to as nodes.
- One of the key benefits associated with the operation of a docker swarm is the high level of availability offered for applications.
- Docker Swarm lets you connect containers to multiple hosts similar to Kubernetes.
- Docker Swarm has two types of services: replicated and global.
```
docker swarm init 
```
Initialize a swarm. The Docker Engine targeted by this command becomes a manager in the newly created single-node swarm.



## Docker Swarm Stacks:
- When running Docker Engine in swarm mode, you can use `docker stack deploy` to deploy a complete application stack to the swarm. 
- The `deploy` command accepts a stack description in the form of a [﻿Compose file](https://docs.docker.com/compose/compose-file/legacy-versions/).


## Docker Healthchecks:
- HEALTHCHECK was added in 1.12
- Supports in Dockerfile, Compose YAML, Docker run, and swarm services.
- The Docker engine will exec command in the container
- It expects exit(0) Ok or exit(1) error
- Three container states: starting healthy unhealthy
- not a external monitoring replacement


# Kubernetes:
Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. 

- Popular container orchestration
- container orchestration = make many servers act like one
- released by Google in 2015, managed by an open-source community
- runs on top of docker (usually) as a set of APIs in the containers
- provides API/CLI to manage containers across servers
- many cloud provider gives it 
- many vendors makes a "distribution" of it 


## Swarm vs Kubernetes:
### Advantages of Swarm:
- Comes with docker, single vendor container platform
- Easier to deploy and manage
- Follows 80/20 rule 20% of features of Kubernetes and solves 80% of problems 
- Runs where docker runs: such as local, cloud, datacenter
- arm windows 32 bit
- secure by default
- easier to troubleshoot
recommended for small teams 



### Advantages of Kubernetes:
- Clouds will deploy/manage Kubernetes for you
- infrastructure vendors are making their own distributions
- Widest adoption and community
- flexible: covers widest set of use cases
- "Kubernetes first" vendor support
- Trending will benefit in your career




## Basic Terminology in Kubernetes:
- Kubernetes - The whole orchestration system.
- k8s- for short 
- Kubectl: a command line tool for communicating with a Kubernetes cluster's control plane, using the Kubernetes API. using cuber control official name
-  Node: single server in kubernetes cluster
- kubelet : kuernetes agent running on nodes
- Control plane: set of containers that manages the cluster


## Kubernetes Container Abstraction:
#### Kubernetes Pods:
 A Pod is a Kubernetes abstraction that represents a group of one or more application containers (such as Docker), and some shared resources for those containers. Those resources include:

- Shared storage, as Volumes
- Networking, as a unique cluster IP address
- Information about how to run each container, such as the container image version or specific ports to use
 For example, a Pod might include both the container with your Node.js app as well as a different container that feeds the data to be published by the Node.js webserver.

Pods are the atomic unit on the Kubernetes platform. When we create a Deployment on Kubernetes, that Deployment creates Pods with containers inside them (as opposed to creating containers directly).



#### Nodes:
- A Pod always runs on a **Node**. A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster.
- Each Node is managed by the control plane. 
- A Node can have multiple pods, and the Kubernetes control plane automatically handles scheduling the pods across the Nodes in the cluster. 
- Every Kubernetes Node runs at least:
1.  Kubelet, a process responsible for communication between the Kubernetes control plane and the Node; it manages the Pods and the containers running on a machine.
2. A container runtime (like Docker) responsible for pulling the container image from a registry, unpacking the container, and running the application.
#### Troubleshooting with kubectl:
- **kubectl get** - list resources
- **kubectl describe** - show detailed information about a resource
- **kubectl logs** - print the logs from a container in a pod
- **kubectl exec** - execute a command on a container in a pod
We can use these commands to see when applications were deployed, what their current statuses are, where they are running and what their configurations are.



#### Creating Pods in Kubernetes:
```
kubectl run my-nginx --image nginx
```
This will create a pod with docker desktop.

- Unlike docker, we cant directly create containers in kubernetes.
- We have to create pods using CLI, YAML, or API
- kubernetes then create containers inside that.
- Kubelet tells the container runtime to create containers for us
- Every resource that we use uses pod to create containers.


```
kubectl get deploy/my-apache -o yaml
```
```
{
  "apiVersion": "apps/v1",
  "kind": "Deployment",
  "metadata": {
    "name": "my-apache",  // Name of the deployment
    "namespace": "default",  // Namespace of the deployment
    "creationTimestamp": "2024-06-26T09:25:16Z"  // Creation time of the deployment
  },
  "spec": {
    "replicas": 2,  // Desired number of replicas
    "selector": {
      "matchLabels": {
        "app": "my-apache"  // Label selector for the deployment
      }
    },
    "strategy": {
      "type": "RollingUpdate",  // Deployment strategy type
      "rollingUpdate": {
        "maxSurge": "25%",  // Maximum number of pods that can be created over the desired number of pods
        "maxUnavailable": "25%"  // Maximum number of pods that can be unavailable during the update process
      }
    },
    "template": {
      "metadata": {
        "labels": {
          "app": "my-apache"  // Labels for the pod template
        }
      },
      "spec": {
        "containers": [
          {
            "name": "httpd",  // Name of the container
            "image": "httpd",  // Docker image for the container
            "imagePullPolicy": "Always"  // Image pull policy
          }
        ],
        "restartPolicy": "Always",  // Restart policy for the pod
        "terminationGracePeriodSeconds": 30  // Termination grace period in seconds
      }
    }
  },
  "status": {
    "replicas": 2,  // Total number of replicas
    "updatedReplicas": 2,  // Number of updated replicas
    "readyReplicas": 2,  // Number of ready replicas
    "availableReplicas": 2,  // Number of available replicas
    "conditions": [
      {
        "type": "Available",
        "status": "True",  // Availability status
        "lastUpdateTime": "2024-06-26T09:35:21Z",  // Last update time for the condition
        "reason": "MinimumReplicasAvailable",  // Reason for the condition
        "message": "Deployment has minimum availability."  // Message for the condition
      }
    ]
  }
}
```
##  Service Types in Kubernetes
> I**n Kubernetes, a Service is a method for exposing a network application that is running as one or more **[**﻿Pods**](https://kubernetes.io/docs/concepts/workloads/pods/)** in your cluster.**

```
apiVersion: v1                   # API version of the Kubernetes resource
kind: Service                    # Type of Kubernetes resource (in this case, a Service)
metadata:
  name: my-service               # Name of the Service resource
spec:
  selector:                      # Selector to determine which Pods will belong to this Service
    app.kubernetes.io/name: MyApp  # Label selector to match Pods with the label "app.kubernetes.io/name: MyApp"
  ports:                         # Ports that the Service exposes
    - protocol: TCP              # Protocol used (TCP in this case)
      port: 80                   # Port on the Service itself
      targetPort: 9376           # Port on the Pods that the Service forwards traffic to
```
### **ClusterIP Services:**
- ClusterIP is the default service type in Kubernetes, and it provides internal connectivity between different components of our application.
- Kubernetes assigns a virtual IP address to a ClusterIP service that can solely be accessed from within the cluster during its creation
- This IP address is stable and doesn’t change even if the pods behind the service are rescheduled or replaced.
- To create a ClusterIP service in Kubernetes, we need to define it in a YAML file and apply it to the cluster. Here’s an example of a simple ClusterIP service definition:
```
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector:
    app: backend
  ports:
  - name: http
    port: 80
    targetPort: 8080
```
### **NodePort Service**
- NodePort services extend the functionality of ClusterIP services by enabling external connectivity to our application.
- When we create a _NodePort _service on any node within the cluster that meets the defined criteria, **Kubernetes opens up a designated port that forwards traffic to the corresponding **_**ClusterIP **_**service running on the node.**
- These services are ideal for applications that need to be accessible from outside the cluster, such as web applications or APIs
- With _NodePort _services, we can access our application using the node’s IP address and the port number assigned to the service.
```
apiVersion: v1 
kind: Service 
metadata: 
  name: frontend 
spec: 
  selector: 
    app: frontend 
  type: NodePort 
  ports: 
    - name: http 
      port: 80 
      targetPort: 8080
```
### LoadBalancer Services:
- LoadBalancer services are ideal for applications that need to handle high traffic volumes, such as web applications or APIs. 
- When we create a _LoadBalancer _service, **Kubernetes provisions a load balancer in our cloud environment and forwards the traffic to the nodes running the service.**
```
apiVersion: v1
kind: Service
metadata:
  name: web
spec:
  selector:
    app: web
  type: LoadBalancer
  ports:
    - name: http
      port: 80
      targetPort: 8080
```
After creating the _LoadBalancer _service, Kubernetes provisions a load balancer in the cloud environment with a public IP address. We can use this IP address to access our application from outside the cluster.



### Creating a NodePort Service:
```
﻿kubectl expose deployment/<deployment-name> --port <port-number> --name <service-name> --type <service-type> 
```
```
kubectl expose deployment/httpenv --port 8888 --name htttpenv-np --type NodePort
```
This command sets up a Kubernetes service named `httpenv-np` to expose the `httpenv` deployment on port 8888 using the `NodePort` service type. This allows external access to the `httpenv` application via any node's IP address on the specified NodePort.







### Imperative vs. declarative Kubernetes commands
There are two ways you can configure a resource under Kubernetes: imperatively or declaratively.

- [ ] Imperative configuration means that to describe the configuration of the resource, you execute a command from a terminal’s command prompt.
- [ ] Declarative configuration means that you create a file that describes the configuration for the particular resource and then apply the content of the file to the Kubernetes cluster.
- [ ] To apply the configuration, you use the command `kubectl apply`  at the command prompt as you would for an imperative command, but that’s where the similarity ends.
 To create a pod** imperatively**, execute the `kubectl run` command set in a terminal window:

```
kubectl run nicepod --image=nginx
```
To create a pod declaratively, you create a manifest file, either in JSON or YAML. Once the file is created, execute the command set `kubectl apply -f <FILENAME>` from the command prompt.

The following example is a manifest file in YAML, named _nicepod.yaml_, that creates a pod with the name _nicepod_.

```
apiVersion: v1 # API version of the Kubernetes object
kind: Pod # Kind of the object (Pod in this case)
metadata:
  name: nicepod # Name of the Pod (any valid Pod name)
  labels:
    App: dev # Label key and value (key: App, value: dev)
spec:
  containers:
    - name: web # Name of the container (any valid container name)
      image: nginx # Image to be used for the container (container image, e.g., nginx)
      ports:
        - name: web # Name of the port (any valid port name)
          containerPort: 80 # Port number on the container (any valid port number, e.g., 80)
          protocol: TCP # Protocol for the port (protocol type, e.g., TCP)
```
To create the source, run the following command in a terminal window:

```
﻿kubectl apply -f nicepod.yaml 
```
The command above essentially says, “Apply this declaration defined in the file _nicepod.yaml_ to create the resource within the Kubernetes cluster.”

Once the YAML file is applied, the resource is created. To verify all is as intended, execute the command `kubectl get pods`. You’ll see the following output:

