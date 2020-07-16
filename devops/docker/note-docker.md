### Docker Note



### Installation

Double-click **Docker Desktop for Windows Installer.exe** to run the installer.

Follow the install wizard to accept the license, authorize the installer, and proceed with the install.

Click **Finish** on the setup complete dialog to launch Docker.

### Start Docker Desktop for Windows

Docker does not start automatically after installation. To start it, search for Docker, select **Docker Desktop for Windows** in the search results, and click it (or hit Enter).

### Run

Open a command-line terminal like PowerShell, and try out some Docker commands!

Run `docker version` to check the version.

Run `docker run hello-world` to verify that Docker can pull and run images.



---



### I. Basic

1\. Installation

```shell
docker --version
```

2\. Switch to Windows container if you using Window, vice versa.

3\. Run hello-world Docker image

```shell
$ docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

4\. Knowing basic commands of Docker

```shell
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Execute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq
```



### II. Building an App by Docker

Hierarchy

- Stack
- Services
- Container

Ensuring that your app, its dependencies, and the runtime, all travel together.

#### Define a Container with Dockerfile

Write a Dockerfile

```shell
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

requirements.txt

```shell
Flask
Redis
```

app.py

```shell
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

Switch Linux Container

Build the app

```shell
# cd 
$ ls
Dockerfile		app.py			requirements.txt
# build
$ docker build --tag=friendlyhello .
# your built image
$ docker image ls
```

Run the app

```shell
$ docker run -p 4000:80 friendlyhello
```

Visit app

visit localhost:4000.

or 

```shell
$ curl http://localhost:4000

<h3>Hello World!</h3><b>Hostname:</b> 8fc990912a14<br/><b>Visits:</b> <i>cannot connect to Redis, counter disabled</i>
```

Docker container stop

```shell
# List all running containers
$docker container ls
# List all containers, even those not running
$docker container ls -a
# docker container stop <hash>
# Gracefully stop the specified container
$ docker container stop 1fa4ab2cf395
```

#### Share your image

Log in with your Docker ID

```shell
$ docker login
```

Tag the image

```shell
# docker tag image username/repository:tag
$ docker tag friendlyhello gordon/get-started:part2
# see your tag of image
$ docker image ls
```

Publish the image

```shell
# docker push username/repository:tag
$ docker push gordon/get-started:part2
```

Pull and run the image from the remote repository

```shell
# docker run -p 4000:80 username/repository:tag
$ docker run -p 4000:80 gordon/get-started:part2
```



### III. Services

A distributed application: the **service**.

#### Run your new load-balanced app

Init swarm

```shell
$ docker swarm init
```

deploy

```shell
# docker stack deploy -c <docker-compose.yml> <app_name>
$ docker stack deploy -c docker-compose.yml getstartedlab
```

List all services 

```shell
$ docker service ls
```

Start a service by service_name

```shell
# docker service ps <service_name>
$ docker service ps getstartedlab_web
```

Show up all containers not filtered by service.

```shell
$ docker container ls -q
# visit http://localhost:4000 or curl
$ curl -4 http://localhost:4000
```

View all tasks of a stack

```shell
$ docker stack ps
```

#### Scale the app

You can scale the app by changing the `replicas` value in `docker-compose.yml`, saving the change, and re-running the `docker stack deploy` command:

```shell
# docker stack deploy -c <docker-compose.yml> <app_name>
$ docker stack deploy -c docker-compose.yml getstartedlab
```

#### Take down the app and the swarm

```shell
# docker stack rm <app_name>
$ docker stack rm getstartedlab
$ docker swarm leave --force
```



### IV. Swarms

You deploy this application onto a cluster, running it on multiple machines. Multi-container, multi-machine applications are made possible by joining multiple machines into a “Dockerized” cluster called a **swarm**.

- **swarm manager**
- **nodes**
- **workers**
- **swarm mode**

#### Set up your swarm

A swarm is made up of multiple nodes, which can be either physical or virtual machines. The basic concept is simple enough: run `docker swarm init` to enable swarm mode and make your current machine a swarm manager, then run `docker swarm join` on other machines to have them join the swarm as workers. Choose a tab below to see how this plays out in various contexts. We use VMs to quickly create a two-machine cluster and turn it into a swarm.



### V. Stacks

Make multiple services relate to each other, and run them on multiple machines.



### VI. Deploy Your App