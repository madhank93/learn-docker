# Docker notes

## History

![history](/img/history.png)

## What is docker

**Docker** is a tool designed to make it easier to create, deploy, and run applications by using containers.

## Why do you need docker

![compatibility_dependency_issue](/img/compatibility_dependency_issue.jpg)

* To avoid compatibility issues with an underlying OS or between the services & libraries dependencies with the OS. (So no more - It works on my machine!)

* To reduce local development environment setup time.

* Whenever your app needs to go through multiple phases dev/test/uat/prod (to operate as same on all the platforms).

* When you want to adopt a microservices architecture.

## What can docker do

![docker_ability](/img/docker_ability.jpg)

* Containerize an applications

* Isolates apps from each other

* Run each service with its own dependencies in separate containers

## What are containers and images

**Container** allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. Its decouples the OS from the application dependencies and the code. It is a completely isolated environment with their own processes, network interfaces and their own mounts except they all share the same OS kernel.

An **image** is a package or a template, it is used to create one or more containers. Containers are running instance of images.

## Image vs Container

```text
- Image is the application we want to run
- Container is an instance of that image running as a process
```

## How does docker works

![docker-engine](/img/docker-engine-components-flow.png)

Docker Engine is a client-server application with these major components:

* A server which is a type of long-running program called a daemon process (the dockerd command).

* A REST API which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.

* A command line interface (CLI) client (the docker command).

The CLI uses the Docker REST API to control or interact with the Docker daemon through scripting or direct CLI commands. Many other Docker applications use the underlying API and CLI. The daemon creates and manages Docker objects, such as images, containers, networks, and volumes.

## Docker command line structure (format)

```text
New way: docker <command> <sub-commands> (options)
Old way: docker <command> (options)

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Example:
New way - docker container run
Old way - docker run
```

## Creating and using containers

<details>

  <summary>To check your docker version</summary>

  <p>

```docker
docker version
```

  </p>

</details>

---

<details>

  <summary> To check your docker info (shows most config values of the engine) </summary>

  <p>

```docker
docker info
```
  </p>

</details>

---

<details>

  <summary>To pull docker images</summary>

  <p>

Syntax:

```docker
docker pull name:tag
```

Example:

```docker
docker pull nginx:latest
docker pull nginx:1.19.6
```

  </p>

</details>

---

<details>

  <summary>To pull private docker images</summary>

  <p>

Syntax:

```docker
docker login
docker pull name:tag
```

Example:

```docker
docker login
docker pull madhank93/wdio
```

To access private images you need to authenticate at first.

  </p>

</details>

---

<details>

  <summary>To list local docker images</summary>

  <p>

Syntax:

```docker
docker images
```

Result:
```
REPOSITORY              TAG       IMAGE ID       CREATED        SIZE
nginx                   latest    f6d0b4767a6c   2 weeks ago    133MB
```

  </p>

</details>

----

<details>

  <summary> To start a docker container </summary>

  <p>

```docker
docker container start nginx
```

  </p>

#### run vs start

`run` always starts a *new* container;
      if the image is not locally available, it automatically pulls the image and starts running it. 

`start` starts an existing stopped one

</details>

-----

<details>

  <summary> To run a docker container in a foreground </summary>

  <p>

```docker
docker container run --publish 4000:80 nginx
```
  

On execution

* Looks for that image locally in image cache, does not find anything
* Then looks for the image in remote repository (default - docker hub)
* Downloads the latest version by default
* Creates a container based on that image
* Opened port 4000 port on the host IP
* Routes that traffic to container IP, port 80
* Go to localhost:4000 in the browser to see the nginx up and running

`--publish` or `-p` to map a host port to a running container port

Note: publish port format HOST:CONTAINER

  </p>

</details>

---

<details>

  <summary> To list a running docker container </summary>

  <p>

```docker
docker container ls
```

```docker
docker container ps
```

Output of the above command has the container ID and container name

```text
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                   PORTS                NAMES
85861b9fdf01        nginx               "/docker-entrypoint.…"   12 seconds ago      Up 10 seconds       0.0.0.0:80->80/tcp        server
```

`ps` and `ls` both does the same thing, where as `ls` command introduced later (newer version)

  </p>

</details>

---

<details>

  <summary> To list all the docker containers (including stopped containers) </summary>

  <p>

```docker
docker container ls -a
```

`-a` lists out all of the containers

  </p>

</details>

---

<details>
  
  <summary> To stop a docker container </summary>

  <p>

```docker
docker container stop container_name_or_id
```
  </p>

</details>

---

<details>
  
  <summary> To run a docker container in a background </summary>

  <p>

```docker
docker container run --publish 4000:80 --detach nginx
```

```docker
docker container run --publish 4000:80 -d nginx
```

`--detach` or `-d` runs the container in background mode

  </p>

</details>

---

<details>
  
  <summary> To give docker container a name </summary>

  <p>

```docker
docker container run --publish 4000:80 -- detach --name webserver nginx
```

`--name` gives the container a name

  </p>

</details>

---

<details>
  
  <summary> To see the logs (if you run the container in background and want to see the logs) </summary>

  <p>

```docker
docker container logs container_name_or_id
```

  </p>

</details>

---

<details>

  <summary> To remove the container </summary>

  <p>

```docker
docker container rm container_name_or_id
```

  </p>

</details>

---

<details>

  <summary>To force remove the container</summary>

  <p>

* To force remove the container(even if it is running)

```docker
docker container rm -f container_name_or_id
```

`-f` force removes the container

*Note* : You cannot remove the running container. Either you can stop the container and remove it or force remove the container

  </p>

</details>

---

<details>

  <summary> To list running process in specific container </summary>

  <p>

```docker
docker top container_name_or_id
```

  </p>

</details>

---

<details>

  <summary> Assignment: Manage multiple containers </summary>

  <p>

```docker
docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql

docker container logs db // to get the generated random password from the log

docker container run -d --name server -p 8080:80 httpd

docker container run -d --name proxy -p 80:80 nginx
```

*Note* : Just because the containers(httpd, and nginx) are both listening on port 80 inside (the right number), there is no conflict because on the host they are published on 80, and 8080 separately (the left number).

![host_container_port](/img/host_container_port.png)

  </p>

</details>

---

<details>

  <summary> Docker CLI Process monitoring </summary>

  <p>

```docker
docker container top container_name_or_id // process list in one container
docker container inspect container_name_or_id // details of one container config; meta data about the container (startup config, volumes, networking ...)
docker container stats container_name_or_id // performance stats for all container (shows live performance)
```

  </p>

</details>

---

<details>

  <summary> Getting a Shell inside a container </summary>

  <p>

1. Getting a shell inside a new container (starts new container interactively)

  ```docker
  docker container run -it --name proxy nginx bash
  ```

  `i` interactive (keeping session open to receive input)

  `t` pseudo-tty (simulates a real terminal)

  `bash` run with `-it` to give a running terminal inside the container

2. Getting a shell inside a existing container (run additional command in existing container)

  ```docker
  docker container exec -it container_name_or_id bash
  ```

  </p>

</details>

---

<details>

   <summary> Docker network concepts </summary>

   <p>

```docker
docker container port container_name_or_id
```

`port` exposes the which ports are forwarding traffic to that container from the host

```docker
docker container inspect --format "{{ .NetworkSettings.IPAddress }}" container_name_or_id
```

`--format` formats the output

  </p>

</details>

----

## Resources

* [Katacoda docker](https://www.katacoda.com/courses/container-runtimes)

* [Play with docker](https://labs.play-with-docker.com/)

* [Docker cheat sheet](http://dockerlabs.collabnix.com/docker/cheatsheet/)

* [Docker Tutorial for Beginners - TechWorld with Nana](https://www.youtube.com/watch?v=3c-iBn73dDE&ab_channel=TechWorldwithNana)

* [Docker Tutorial for Beginners - 	Mmumshad](https://www.youtube.com/watch?v=fqMOX6JJhGo&ab_channel=freeCodeCamp.org)

## References

[1] <https://docs.docker.com/get-started/>

[2] <https://opensource.com/resources/what-docker>

[3] <https://docs.microsoft.com/en-us/dotnet/architecture/microservices/container-docker-introduction/docker-terminology>

[4] <https://www.redhat.com/en/topics/containers/what-is-docker>

[5] <https://github.com/collabnix/dockerlabs>
