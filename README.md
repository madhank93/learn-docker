# Docker notes

## History

![history](/img/history.png)

## What is docker ?

**Docker** is a tool designed to make it easier to create, deploy, and run applications by using containers.

## Why do you need docker ?

![compatibility_dependency_issue](/img/compatibility_dependency_issue.jpg)

* To avoid compatibility issues with an underlying OS or between the services & libraries dependencies with the OS. (So no more - It works on my machine!)

* To reduce local development environment setup time.

* Whenever your app needs to go through multiple phases dev/test/uat/prod (to operate as same on all the platforms).

* When you want to adopt a microservices architecture.

## What can docker do ?

![docker_ability](/img/docker_ability.jpg)

* Containerize an applications

* Isolates apps from each other

* Run each service with its own dependencies in separate containers
## What are containers and images ?

**Container** allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. Its decouples the OS from the application dependencies and the code. It is a completely isolated environment with their own processes, network interfaces and their own mounts except they all share the same OS kernel.

An **image** is a package or a template, it is used to create one or more containers. Containers are running instance of images.

## Image vs Container

```text
- Image is the application we want to run
- Container is an instance of that image running as a process
```

## How does docker works ?

![docker-engine](/img/docker-engine-components-flow.png) 

<img src="https://github.com/madhank93/learn_docker/blob/master/img/dokcer_client_server.png" width="500" height="500">

Docker Engine is a client-server application with these major components:

* A server which is a type of long-running program called a daemon process (the dockerd command).

* A REST API which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.

* A command line interface (CLI) client (the docker command).

The CLI uses the Docker REST API to control or interact with the Docker daemon through scripting or direct CLI commands. Many other 
Docker applications use the underlying API and CLI. The daemon creates and manages Docker objects, such as images, containers, 
networks, and volumes.

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

<details>

  <summary> 1. How to check your docker version ? </summary>

  <p>

```docker
docker version
```

  </p>

</details>

---

<details>

  <summary> 2. How to check your docker info (shows most config values of the engine) ? </summary>

  <p>

```docker
docker info
```
  </p>

</details>

---

<details>

  <summary> 3. How to pull docker images ? </summary>

  <p>

Syntax:

```docker
docker pull <image-name>:<tag>
```

**Note**: If tag is not specified by default it takes latest

Example:

```docker
docker pull nginx:latest
docker pull nginx:1.19.6
```

  </p>

</details>

---

<details>

  <summary> 4. How to pull private docker images ? </summary>

  <p>

Syntax:

```docker
docker login
docker pull <image-name>:<tag>
```

**Note:** To access private images you need to authenticate at first.

Example:

```docker
docker login
docker pull madhank93/wdio
```

  </p>

</details>

---

<details>

  <summary> 5. How to list local docker images ? </summary>

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

  <summary> 6. How to start a docker container ? </summary>

  <p>

Syntax:

```docker
docker container start <container-id-or-name>
```

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

  <summary> 7. How to run a docker container in a foreground ? </summary>

  <p>

Syntax:

```docker
docker container run <image-id-or-name>
```

Example:

```docker
docker container run --publish 4000:80 nginx
```


**On execution:**

* Looks for that image locally in image cache, does not find anything
* Then looks for the image in remote repository (default - docker hub)
* Downloads the latest version by default
* Creates a container based on that image
* Opened port 4000 port on the host IP
* Routes that traffic to container IP, port 80
* Go to localhost:4000 in the browser to see the nginx up and running

`--publish` or `-p` to map a host port to a running container port

**Note**: publish port format HOST:CONTAINER

  </p>

</details>

---

<details>

  <summary> 8. How to list a running docker container ? </summary>

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

  <summary> 9. How to list all the docker containers (including stopped containers) ? </summary>

  <p>

```docker
docker container ls -a
```

`-a` lists out all of the containers

  </p>

</details>

---

<details>
  
  <summary> 10. How to stop a docker container ? </summary>

  <p>

Syntax:

```docker
docker container stop <container-id-or-name>
```

Example:

```docker
docker container stop nginx
```

  </p>

</details>

---

<details>
  
  <summary> 11. How to run a docker container in a background ? </summary>

  <p>

Syntax:

```docker
docker container run -d <container-id-or-name>
docker container run --detach <container-id-or-name>
```

Example:

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
  
  <summary> 12. How to give docker container a name ? </summary>

  <p> 

Syntax:

```docker
docker container run --name <container-name> <container-id-or-name>
```

Example:

```docker
docker container run --publish 4000:80 -- detach --name webserver nginx
```

`--name` gives the container a name

  </p>

</details>

---

<details>
  
  <summary> 13. How to see the logs (if you run the container in background and want to see the logs) ? </summary>

  <p>

Syntax:

```docker
docker container logs <container-id-or-name>
```

Example:

```docker
docker container logs nginx
```

  </p>

</details>

---

<details>

  <summary> 14. How to remove the container ? </summary>

  <p>

Syntax:

```docker
docker container rm <container-id-or-name>
```

**Note**: This command will only remove the stopped container

Example:

```docker
docker container rm nginx
```

  </p>

</details>

---

<details>

  <summary> 15. How to force remove the container ? </summary>

  <p>

* To force remove the container(even if it is running)

Syntax:

```docker
docker container rm -f <container-id-or-name>
docker container rm --force <container-id-or-name>
```

Example:

```docker
docker container rm -f nginx
```

`-f` or `--force` force removes the container

*Note* : You cannot remove the running container. Either you can stop the container and remove it or force remove the container

  </p>

</details>

---

<details>

  <summary> 16. How to list running process in specific container ? </summary>

  <p>

```docker
docker top <container-id-or-name>
```

  </p>

</details>

---

<details>

  <summary> 17. How to manage multiple containers ? </summary>

  <p>

```docker
docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql

docker container logs db // to get the generated random password from the log

docker container run -d --name server -p 8080:80 httpd

docker container run -d --name proxy -p 80:80 nginx
```

*Note* : Just because the containers(httpd, and nginx) are both listening on port 80 inside (the right number), there is no conflict 
because on the host they are published on 80, and 8080 separately (the left number).

![host_container_port](/img/host_container_port.png)

  </p>

</details>

---

<details>

  <summary> 18. How to monitor docker process from CLI ? </summary>

  <p>

```docker
docker container top <container-id-or-name> // process list in one container
docker container inspect <container-id-or-name> // details of one container config; meta data about the container (startup config, volumes, networking ...)
docker container stats <container-id-or-name> // performance stats for all container (shows live performance)
```

  </p>

</details>

---

<details>

  <summary> 19. How to get a Shell inside a container ? </summary>

  <p>

Syntax:

```docker
docker container exec -it <container-id-or-name> <command-name>
```

1. Getting a shell inside a new container (starts new container interactively)

  ```docker
  docker container run -it --name proxy nginx bash
  ```

  `i` interactive (keeping session open to receive input)

  `t` pseudo-tty (simulates a real terminal)

  `bash` run with `-it` to give a running terminal inside the container

2. Getting a shell inside a existing container (run additional command in existing container)

  ```docker
  docker container exec -it <container-id-or-name> bash
  ```

  </p>

</details>

---

<details>

   <summary> 20. Docker network concepts </summary>

   <p>

```docker
docker container port <container-id-or-name>
```

`port` exposes the which ports are forwarding traffic to that container from the host

```docker
docker container inspect --format "{{ .NetworkSettings.IPAddress }}" <container-id-or-name>
```

`--format` formats the output

  </p>

</details>

----

<details>

   <summary> 21. What is layers in docker images ? </summary>

   <p>

Images are composed of layers. Each layer is a set of filesystem changes. Images are created using a dockerfile and every line in a 
dockerfile results in creating a new layer. 

Every layer gets its own unique SHA number that helps system to identify if that layer has already exists (so that we don't have to 
download the layers that already exists). This guarantees layer are not stored more than one.

If you want to see the layers of the image.

Syntax:

```docker
docker image history <image-id-or-name>
```

Example:

```docker
docker image history redis
```

  </p>

</details>

----

<details>

   <summary> 22. How to tag an existing image ?</summary>

   <p>

Syntax:

```docker
docker image tag <source-image-id> <TARGET_IMAGE>:<TAG>
```

**Note**: If no tag has mentioned by default it will assign latest to it.

Example:

```docker
docker image tag alpine madhank93/alpine:1.0.12
```

  </p>

</details>

----

<details>

   <summary> 23. How to build an image ?</summary>

   <p>

Docker image is built from the `Dockerfile` (Example of the dockerfile is available at `docker-files/creating_img`)

Syntax:

1. If the Dockerfile file is in the root directory (from where you run the command)

```docker
docker image build -t <image-name:tag> .
```

2. If the dockerfile is not present in root directory but at a different folder

```docker
docker image build -f <path-of-the-dockerfile> -t <image-name> .

or 

docker image build --file <path-of-the-dockerfile> -t <image-name:tag> .
```

Example:

```docker
docker image build -f docker-files/creating_img/Dockerfile -t custom_python_img:1.0.0 .
```

**Note**: The order in the `Dockerfile` is important, less changes should be on top and things could change frequently should be 
placed below (like copying the code). So that whenever we are re-building the image, we only rebuild it from that line, otherwise 
docker will use the cached layer.

  </p>

</details>

----

<details>

  <summary> 24. How to see the disk usage of docker ? </summary>

  <p>

Syntax:

```docker
docker system df
```

Output:

![docker_usage](/img/docker_usage.png)

  </p>

</details>

----

<details>

  <summary> 25. How to clean up volumes, build cache, stopped images and containers ? </summary>

  <p>

Syntax:

```docker
docker image prune # to clean up just "dangling" images
docker container prune # to clean up stopped containers
docker system prune # will clean up everything
```

**Note**: Add `-a` to force delete all.

  </p>

</details>

----

<details>

  <summary> 26. Why we need to persist data in docker ? </summary>

  <p>
Docker containers are ephemeral (lasts only for a short period of time), once the container crashes or removed, data (ex: mysql data 
or logs of the server) inside the container will lost. To avoid such scenario, data must be persisted.
  </p>

</details>

----

<details>

  <summary> 27. What are the ways we can persist data ? </summary>

 <p>

  ![types_of_mount](/img/types-of-mounts.png)

  There is 2 ways,

    1. Data volumes - are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). 
    Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.

       a. Anonymous volume : It can be difficult to refer to this volume later, since docker gives them a random name. 
       b. Named volumes : It lot more easier to refer, since we are naming the volumes.

       ![named_and_anonymous_volume](/img/listing_volume.png.png)

    2. Bind mounting - may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker 
    processes on the Docker host or a Docker container can modify them at any time.

  

  **Note**: For more info refer to [Manage data in docker](https://docs.docker.com/storage/)

  </p>

</details>

----

<details>

  <summary> 28. How to create the data volumes in docker ? </summary>

<p>

  1. Anonymous volume:
     
  Syntax:

  ```docker
    docker container run -v <path-in-container> <image-id-or-name>
  ```

  Example:

  ```docker
    docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v /var/lib/mysql mysql
  ```

  2. Named volume:

  Syntax:

  ```docker
    docker container run -v <volume-name>:<path-in-container> <image-id-or-name>
  ```

  Example:

  ```docker
    docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql  mysql
  ```

  </p>

</details>

----

<details>

  <summary> 29. How to bind mounting in docker ? </summary>

  <p>

  Syntax:

  ```docker
    docker container run -v <path-in-host>:<path-in-container> <image-id-or-name>
  ```
  

  Example:

  ```docker
    docker container run -p 8180:80 -d -v /Users/madhan/Desktop/nginx-logs:/var/log/nginx nginx
  ```

  </p>

</details>

-----

<details>

  <summary> 30. How to migrate to an latest version of docker image without any data ? </summary>

  <p>

  Example: 

  ```docker
  docker container run -d --name postgres -v postgres-db:/var/lib/postgresql/data postgres:9.6.1 # initial version

  docker container run -d --name postgres2 -v postgres-db:/var/lib/postgresql/data postgres:9.6.2 # upgraded to newer version
  ```

  </p>

</details>

-----

<details>

  <summary> 31. How to list out the available networks ?</summary>

  <p>

  Syntax:

  ```docker
  docker network ls
  ```

  </p>

</details>

----

<details>

  <summary> 32. What are all the default networks available in docker ?</summary>

  <p>

  There are 3 default networks available. They are,

![docker_network](/img/docker_networks.png)

    1. Bridge
    2. none
    3. host

  </p>

</details>

----

<details>

  <summary> 33. How does the containers communicate with other containers ?</summary>

  <p>

 When two containers are connected to the same user-defined bridge network, one container is able to connect to an another container by using its container/service name (as the hostname). When you run containers using docker-compose, it will automatically creates one.

  </p>

</details>

----

<details>

  <summary> 34. What is the relation between Dockerfile and docker compose yml file ?</summary>

  <p>

  Dockerfile - is used to create an docker image out of it

  Docker-compose yml - it is used to easily run the (multi)containers, docker commands are easily maintained.
  
  </p>

</details>

----

<details>

  <summary> 35. Difference between RUN vs CMD vs ENTRYPOINT ?</summary>

  <p>

  RUN - executes command(s) in a new layer and creates a new image on top of an existing image.

  CMD - used to set a default command, can be overwritten from command line when docker container runs.

  Ex: CMD sleep 5

      docker run image_name 10 # sleep 5 will be replaced with sleep 10

  ENTRYPOINT - it is similar to the CMD, 

  </p>

</details>

----

<details>

  <summary> 36. How to manage multi-container or what is the use of docker compose ? </summary>

  <p>

  Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your
  application’s services. Then, with a single command, you create and start all the services from your configuration.

  Template:

  ```YAML
  versions: '3.1'
    services:
      service_name1: 
        image: 
        command:
        environment:
        volumes:
      service_name2:
        image:
        command:
        environment:
        volumes:
      
      volumes:

      networks:
  ```

  `docker-compose.yml`

  ```YAML
  version: '3'
  services:
    web:
      image: nginx
    db:
      image: mysql
      ports:
      - "3306:3306"
      environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_USER=user
      - MYSQL_PASSWORD=password
      - MYSQL_DATABASE=demodb
  ```

  </p>

</details>

-----

<details>

  <summary> 37. How to run the docker compose yml file ? </summary>

  <p>

  Syntax:

  ```docker
  docker-compose up # if the docker-compose.yml is located at the root level
  docker-compose -f docker-files/docker-compose-ex-1/docker-compose.yml up # if the docker-compose yml file is located at different location 
  ```

  </p>

</details>

-----

<details>

  <summary> 38. How to stop the docker compose yml file ? </summary>

  <p>

  Syntax:

  ```docker
  docker-compose down # if the docker-compose.yml is located at the root level
  docker-compose -f docker-files/docker-compose-ex-1/docker-compose.yml down # if the docker-compose yml file is located at different location 
  ```

  </p>

</details>

-----

<details>

  <summary> 39. How to setup an Drupal and Postgres using docker compose ? </summary>

  <p>

    Refer to the `docker-files/docker-compose-ex-2/docker-compose.yml` file.

  </p>

</details>

-----

<details>

  <summary> 40. How to delete a volumes created with docker compose ? </summary>

  <p>

  Syntax:

  ```docker
  docker-compose down -v
  ```

  </p>

</details>

-----

<details>

  <summary> 41. How to build an image using Docker compose ? </summary>

  <p>

  Refer to the `docker-files/docker-compose-ex-3/docker-compose.yml` file.

  `cd into that folder`

  ```docker
  docker-compose up # to start it
  docker-compose down --rmi local # to stop and delete the containers along with it
  ```

  </p>

</details>

-----

<details>

  <summary> 42. How to list all dangling images ? </summary>

  <p>

  ```docker
  docker images -f dangling=true
  ```

  </p>

</details>

-----

<details>

  <summary> 42. How to list all the exited containers ? </summary>

  <p>

  ```docker
  docker container ls -a -f status=exited
  ```

  </p>

</details>

-----

## Resources

* [Katacoda docker](https://www.katacoda.com/courses/container-runtimes)

* [Play with docker](https://labs.play-with-docker.com/)

* [Docker cheat sheet](http://dockerlabs.collabnix.com/docker/cheatsheet/)

* [Docker Tutorial for Beginners - TechWorld with Nana](https://www.youtube.com/watch?v=3c-iBn73dDE&ab_channel=TechWorldwithNana)

* [Docker Tutorial for Beginners - 	Mmumshad](https://www.youtube.com/watch?v=fqMOX6JJhGo&ab_channel=freeCodeCamp.org)

* [Udemy - Docker tutorial - Bret Fisher](https://www.udemy.com/course/docker-mastery/)

* [Docker handbook](https://www.freecodecamp.org/news/the-docker-handbook/)

* <https://docs.docker.com/get-started/>

* <https://opensource.com/resources/what-docker>

* <https://docs.microsoft.com/en-us/dotnet/architecture/microservices/container-docker-introduction/docker-terminology>

* <https://www.redhat.com/en/topics/containers/what-is-docker>

* <https://github.com/collabnix/dockerlabs>
