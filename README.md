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

```docker
docker version
```
</details>

---

<details>

  <summary> To check your docker info (shows most config values of the engine) </summary>

```docker
docker info
```

</details>

---

<details>
  <summary> To run a docker container in a foreground </summary>

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

</details>

---

<details>

  <summary> To start a docker container </summary>

```docker
docker container start nginx
```

#### run vs start

`run` always starts a *new* container

`start` starts an existing stopped one

</details>

---

<details>

  <summary> To list a running docker container </summary>

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

</details>

---

<details>

  <summary> To list all of the docker container </summary>

```docker
docker container ls -a
```

`-a` lists out all of the containers

</details>

---

<details>
  <summary> To stop a docker container </summary>

```docker
docker container stop container_name_or_id
```

</details>

---

<details>
  <summary> To run a docker container in a background </summary>

```docker
docker container run --publish 4000:80 --detach nginx
```

```docker
docker container run --publish 4000:80 -d nginx
```

`--detach` or `-d` runs the container in background mode

</details>

---

<details>
  <summary> To give docker container a name </summary>

```docker
docker container run --publish 4000:80 -- detach --name webserver nginx
```

`--name` gives the container a name

</details>

---

<details>
  <summary> To see the logs (if you run the container in background and want to see the logs) </summary>

```docker
docker container logs container_name_or_id
```

</details>

---

<details>
  <summary> To remove the container </summary>

```docker
docker container rm container_name_or_id
```

</details>

---

<details>
  <summary>To force remove the container</summary>

* To force remove the container(even if it is running)

```docker
docker container rm -f container_name_or_id
```

`-f` force removes the container

*Note* : You cannot remove the running container. Either you can stop the container and remove it or force remove the container

</details>

---

<details>

  <summary> To list running process in specific container </summary>

```docker
docker top container_name_or_id
```

</details>

---

<details>
  <summary> Assignment: Manage multiple containers </summary>

```docker
docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql

docker container logs db // to get the generated random password from the log

docker container run -d --name server -p 8080:80 httpd

docker container run -d --name proxy -p 80:80 nginx
```

*Note* : Just because the containers(httpd, and nginx) are both listening on port 80 inside (the right number), there is no conflict because on the host they are published on 80, and 8080 separately (the left number).

</details>

---

<details>

  <summary> Docker CLI Process monitoring </summary>

```docker
docker container top container_name_or_id // process list in one container
docker container inspect container_name_or_id // details of one container config; meta data about the container (startup config, volumes, networking ...)
docker container stats container_name_or_id // performance stats for all container (shows live performance)
```
</details>

---

<details>
  <summary> Getting a Shell inside a container </summary>

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

</details>

---

<details>
   <summary> Docker network concepts </summary>

```docker
docker container port container_name_or_id
```

`port` exposes the which ports are forwarding traffic to that container from the host

```docker
docker container inspect --format "{{ .NetworkSettings.IPAddress }}" container_name_or_id
```

`--format` formats the output
</details>

----

## Resources

* [Katacoda docker](https://www.katacoda.com/courses/container-runtimes)

* [Play with docker](https://labs.play-with-docker.com/)

* [Docker cheat sheet](http://dockerlabs.collabnix.com/docker/cheatsheet/)

## References

[1] <https://docs.docker.com/get-started/>

[2] <https://opensource.com/resources/what-docker>

[3] <https://docs.microsoft.com/en-us/dotnet/architecture/microservices/container-docker-introduction/docker-terminology>

[4] <https://www.redhat.com/en/topics/containers/what-is-docker>

[5] <https://github.com/collabnix/dockerlabs>
